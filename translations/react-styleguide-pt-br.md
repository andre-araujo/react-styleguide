# WIP - Projeto guia de formatação React

**Este documento esta em construção (WIP) e pode ser modificado conforme necessário**

Projetos [reactJS](https://reactjs.org/) podem escalar muito bem, mas também podem se tornar caóticos muito rapidamente.
Este guia tenta resolver problemas comuns em projetos React grandes.

## Propósito

Criar e documentar um guia [ReactJS](https://reactjs.org/) opinativo para projetos baseados em componentes.

## Contibuindo

Toda ajuda para melhorar esse documento é bem-vinda.

Sugiro abrir uma [issue](https://github.com/andre-araujo/react-styleguide/issues/new), e marcar com as seguintes tags:

- help wanted (perciso de ajuda)
  > Qualquer conceito não compreendido, pergunta ou dúvida.
- opinion (opnião)
  > Qualquer opnião sobre qualquer conceito. 
- enhancement (aprimoramento)
  > ideias para melhorar o documento.
- grammar (gramática)
  > erros de gramática ou palavras com erros ortográficos.

## Índice

1. [Estrutura de pastas](#estrutura-de-pastas)
2. [Componentes](#components)
    1. [Elementos](#elementos)
    2. [Módulos](#modulos)
    3. [Features](#features)
    4. [Sub-componentes](#sub-componentes)
    5. [Componentes de terceiros](#componentes-de-terceiros)
    6. [Reusando a lógica de componentes](#reusando-a-logica-de-componentes)
    7. [Estilização](#estilizacao)
3. [Commits](#commits)

## Estrutura de pastas

Este guia abrange apenas a pasta `/src`.

```
/src
  /components
    /elements
      /NomeDoElemento
        NomeDoElemento.js
        NomeDoElemento.spec.js
        NomeDoElemento.stories.js
        NomeDoElemento.styles.js
        index.js
    /modules
      /NomeDoModulo
        NomeDoModulo.js
        NomeDoModulo.spec.js
        NomeDoModulo.stories.js
        NomeDoModulo.styles.js
        index.js
    /features
      /NomeDaFeature
        NomeDaFeature.js
        NomeDaFeature.spec.js
        NomeDaFeature.stories.js
        NomeDaFeature.styles.js
        index.js
    index.js
  /pages
    /minha-pagina
      ...
    /outra-pagina
      ...
    router.js
  /lib
    /stateManager
      /actions
        ...
      /reduces
        ...
      index.js
    /utils
      ...
  /static
    /styles
      reset.css
      outro-estilo-estatico.css
    /fonts
      minha-fonte.woff
    /images
      minha-imagem.jpg
  index.js
```

**[Voltar para o topo](#indice)**

## Componentes

[react-create-component](https://github.com/andre-araujo/react-create-component) pode ser usado para ajudar na criação de componentes.

Cada componente tem a seguinte estrutura:
```
NomeDoModulo.js
NomeDoModulo.spec.js
NomeDoModulo.stories.js
NomeDoModulo.styles.js
index.js
```

Onde:

- `NomeDoComponente.js`
  - é o próprio componente.
- `NomeDoComponente.spec.js`
  - tem os casos de teste do componente.
- `NomeDoComponente.stories.js`:
  - [Storybook](https://storybook.js.org/), ou qualquer outro tipo de documentação que substitua o ".stories", se necessário.
- `NomeDoComponente.styles.js`:
  - estilo do componente, pode ser qualquer CSS ou solução de CSS em Javascript.
- `index.js`:
  - índice do componente, qualquer High Order Componente deve ser aplicado aqui.

### Elementos

Nesse sistema, os elementos são o menor tipo de componente. Eles devem ser altamanente encapsuláveis, mas sem *state* prórpio, e não devem conter margem, espaçamentos ou qualquer outro estilo que possa interferir com o estilo geral da página.

*Exemplos: botões, inputs, ...*

#### Regras dos elementos:

- Elementos não têm [sub-componentes](#sub-componentes).
  > Por quê? Elementos devem ser pequenos e ter uma única responsabilidade. Se precisar de um sub-componente, então deveria ser um módulo.

- **Elementos não podem ser quebrados**.
  > Por quê? Se um elemento pode ser desmembrado, então não é o menor possível.

- Todo elemento deve repassar props não usadas para o seu elemento raiz.
  > Por quê? Isso aumenta a reusabilidade. Repassar props remanescentes para o componente raiz nos permite mudar atributos HTML, como *data-attributes*, *width*, *height*, ...

- Elementos não devem ter margens.
  > Por quê? Adicinar margem aos elementos os deixa mais difícil de alinhar e reutilizar.

- Elementos devem ser *stateless*.
  > Por quê? Controlar elemtentos *stateful* pode ser bem difícil, por isso elementos devem ser controlados por seus pais.

#### Exemplo de elemento:

```javascript
import React from 'react';
import { string, node } from 'prop-types';

function MyElement({
  name,
  ...props,
}) {
  return (
    <p {...props}>
      Eu sou o elemento {name}!
    </p>
  );
}

MyElement.defaultProps = {
  name: 'default name',
};

MyElement.propTypes = {
  name: string,
};

export default MyElement;
```

### Módulos

Módulos usam elementos para criar componentes mais complexos. Eles devem ter um *state* e devem ser facilmente reutilizáveis. Eles organizam elementos ou outros módulos, adicionando margens e modificações aos elementos, mas não devem ter margens e espaçamentos próprios.

Módulos podem ter [sub-componentes](#sub-componentes).

*Exemplos: Menu, Dropdown, ...*

#### Regras dos módulos:

 - Módulos podem ter [sub-componentes](#sub-componentes).
  > Por quê? Tem alguns módulos que fazem sentido serem divididos em outros componentes conforme eles crescem, mas nem todos os componentes do módulo serão reutilizados em outros lugares, veja a sessão de [sub-componentes](#sub-componentes) para mais detalhes.

- Módulos não devem ter margens.
  > Por quê? Adicinar margem aos módulos os deixa mais difícil de alinhar e reutilizar.

- Módulos devem usar elementos, se possível.
  > Por quê? Elementos devem ser reutilizados o máximo possível para manter a consistência da aplicação, mas os módulos também podem mudar o estilo dos elementos para que se adaptem melhor.

- Módulos podem ser *stateful* quando necessário.
  > Por quê? Módulos devem ser fáceis de ser controlados pelos seus pais, mas, como são mais complexos, às vezes é necessário que tenham um *state*.

- Módulos devem evitar *getDerivedStateFromProps* para manipular seu *state*.
  > Por quê? Leia [este artigo](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html) (em inglês).

#### Exemplo de módulo:

```javascript
import React from 'react';
import { string, node } from 'prop-types';

import { MyElement } from '../..';

function MyModule({
  title,
  children
  ...props,
}) {
  return (
    <section>
      <h1>{title}</h1>
      <MyElement name="My Element Name" />
      {children}
    </section>
  );
}

MyModule.defaultProps = {
  title: 'My title',
  children: null,
};

MyModule.propTypes = {
  title: string,
  children: node,
};

export default MyModule;
```

### Features

Features são componentes complexos, que **deve implementar uma funcionalidade**. Elas usam módulos e elementos para preencher esse requisito, e, também, pode ter sub-pastas para componentes específicos dessa feature.

Features podem estar conectadas com gerenciadores de estado e devem ser independentes de outras features.

** Se o componente não implementa uma funcionalidade, então deveria ser um [módulo](#modules).**

Features podem ter [sub-componentes](#sub-componentes).

*Exemplos: Autenticação, gráficos, busca, ...*

#### Regras de features:

- Features podem ter [sub-componentes](#sub-componentes).
  > Por quê? Features geralmente contêm mais de um componente, por isso existe uma grance chance de que alguns componentes serão criados especificamente para uma feature, veja a sessão de [sub-componentes](#sub-componentes) para mais detalhes.

- Features não devem ter margens.
  > Por quê? Adicinar margem às features as deixa mais difícil de alinhar e reutilizar.

- Features devem usar elementos e módulos, se possível.
  > Por quê? Elemtos e módulos devem ser reutilizados o máximo possível para manter a consistência da aplicação, mas as features também podem mudar o estilo dos módulos e elementos para que se adaptem melhor.

- Features podem ser *stateful*.
  > Por quê? Features são funcionalidades fim a fim. Implementam lógica e interações com o usuário. Veja a sessão [Reusando a lógica de componentes](#reusando-a-logica-de-componentes) para mais detalhes.

#### Exemplo de feature:

```javascript
import React, { Component } from 'react';

import {
  MyElement,
  MyModule,
} from '../..';

class MyFeature extends Component {
  state = {
    didSomething: false,
  }

  doSomeAwesomeThing = () => {
    // Some code here...

    this.setState({
      didSomething: true,
    });
  }

  render() {
    const {
      didSomething,
    } = this.state;

    return (
      <section>
        <MyModule title="Awesome title">
          <MyElement name="Another Name" />
        </MyModule>

        {
          didSomething && 'I did an awesome thing!'
        }

        <button onClick={this.doSomeAwesomeThing}>
          I will do something!
        </button>
      </section>
    );
  }
}

export default MyFeature;
```

### Sub-componentes

*TO DO: Informações complementares e estrutura de pastas*

Para mais informações, veja essa [API de componentes React Native](https://facebook.github.io/react-native/docs/picker), ou [este artigo](https://medium.com/maxime-heckel/react-sub-components-513f6679abed) (ambos em inglês).

**[Voltar para o topo](#indice)**

### Componentes de terceiros

Usar componentes de terceiros pode ser muito útil para evitar "reinventar a roda" e pode reduzir muito tempo gasto escrevendo código.

Porém, às vezes é necessário mudar as dependência do nosso projeto para encaixar as nossas necessidades, e pode ser bem estressante refatorar todo o código atual para usar aquela dependência.

Dica:
Todos os componentes de terceiros devem ser reexportados por outro componente que será usado na sua aplicação.

  > Por quê? Usar componentes de terceiros na sua aplicação inteira, ao invés de usá-los diretamente, pode tornar a refatoração mais difícil. Por isso, recomendo reexportá-las. Assim, você terá apenas um arquivo para refatorar se a API do componente mudar ou for substituída.

#### Exemplo de componente de terceiros:

```javascript
// ./src/components/elements/ClickOut/index.js
import ClickOut from 'react-click-out';

/*
  Prefira exports explícitos.
  Evite: export * from 'algumacoisa'
  quando estiver reexportando um
  componente de terceiros, pois
  pode acusar necessidade de refatoração.
*/
export default ClickOut;
```

```javascript
// ./src/components/modules/MyModule/MyModule.js
import React from 'react';

import { ClickOut, MyElement } from '../..';

function MyModule() {
  return (
    <ClickOut onClick={/* Faz açgo... */}>
      <MyElement name="My Element Name" />
      ...
    </ClickOut>
  );
}

export default MyModule;
```

**[Voltar para o topo](#indice)**

### Reusando a lógica de componentes

*TO DO*

**[Voltar para o topo](#indice)**

### Estilização

*TO DO*

**[Voltar para o topo](#indice)**

## Commits

[`Commitizen's cli`](https://github.com/commitizen/cz-cli) é recomendado para certificar mensagens de commit corretas.

**[Voltar para o topo](#indice)**

## Referências e agradecimentos

Este guia foi fortemente influenciado pelo [atomic design](http://bradfrost.com/blog/post/atomic-web-design/), por Bradfrost.

Outras referências:

- [`Airbnb JavaScript Style Guide`](https://github.com/airbnb/javascript)

- [`Commitizen`](https://github.com/commitizen)

- [`Maxime Heckel's sub-component post`](https://medium.com/maxime-heckel/react-sub-components-513f6679abed)

- [`ReactJS`](https://reactjs.org/)

- [`React Native API`](https://facebook.github.io/react-native/docs/components-and-apis)

- [`React-bits styleguide`](https://github.com/vasanthk/react-bits)

- [`David Gilbertson's ReactJS architecturing post`](https://hackernoon.com/the-100-correct-way-to-structure-a-react-app-or-why-theres-no-such-thing-3ede534ef1ed)

**[Voltar para o topo](#indice)**

## Licença

This document is [MIT licensed](https://github.com/andre-araujo/react-styleguide/blob/master/LICENSE).
