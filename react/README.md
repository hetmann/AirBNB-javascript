# Airbnb React/JSX Ghid de Redactare/Scriere

*O abordare rezonabilă pentru React și JSX*

## Cuprins

  1. [Reguli de bază](#reguli-de-bază)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Denumirea](#denumirea)
  1. [Declarare](#declarare)
  1. [Aliniere](#aliniere)
  1. [Ghilimele](#ghilimele)
  1. [Spațiere](#spațiere)
  1. [Props](#props)
  1. [Paranteze](#paranteze)
  1. [Tag-uri](#tag-uri)
  1. [Metode](#metode)
  1. [Ordonare](#ordonare)
  1. [`isMounted`](#ismounted)

## Reguli de bază

  - Include o singură componentă de React per fișier.
    - Totusi, multiple [Fara state-uri, sau Curate, Componente](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) sunt acceptate per fișier. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - Folosește sintaxa JSX.
  - Nu folosi `React.createElement` numai dacă inițializezi app-ul dintr-un fișier care nu este de tip JSX.

## Class vs `React.createClass` vs stateless

  - Daca există stări (state) și/sau refs interne, folosește `class extends React.Component` în loc de `React.createClass` numai daca există un motiv bun să utilizezi mixin-uri. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // Nerecomandat
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // RECOMANDAT
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    Și dacă nu există stări (state-uri) sau refs, folosește funcții normale (nu funcții arrow) în loc de clase:

    ```jsx
    // Nerecomandat
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // Nerecomandat (relying on function name inference is discouraged)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // RECOMANDAT
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Denumirea

  - **Extensiile**: Folosește extensia `.jsx` pentru componente React.
  - **Fișierul**: Folosește PascalCase pentru fișiere. Ex., `CardRezervare.jsx`.
  - **Denumirea referințelor**: Folosește PascalCase pentru componentele React și camelCase pentru instanțele lor. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // Nerecomandat
    import cardRezervare from './CardRezervare';

    // RECOMANDAT
    import CardRezervare from './CardRezervare';

    // Nerecomandat
    const ArticolRezervare = <CardRezervare />;

    // RECOMANDAT
    const articolRezervare = <CardRezervare />;
    ```

  - **Denumirea Componentei**: Folosește numele fișierului ca și numele componentei. De exemply, `CardRezervare.jsx` trebuie să aiba un nume de referință `CardRezervare`. Totusi, pentru componentele principale dintr-un director/folder, folosește `index.jsx` ca și numele fișierului și numele directorul/folder-ul ca și numele componentei:

    ```jsx
    // Nerecomandat
    import Footer from './Footer/Footer';

    // Nerecomandat
    import Footer from './Footer/index';

    // RECOMANDAT
    import Footer from './Footer';
    ```

## Declarare

  - Nu folosi `displayName` pentru numirea componentelor. În schimb, denumește componenta prin referință.

    ```jsx
    // Nerecomandat
    export default React.createClass({
      displayName: 'CardRezervare',
      // alte informații aici
    });

    // RECOMANDAT
    export default class CardRezervare extends React.Component {
    }
    ```

## Aliniere

  - Urmărește aceste stiluri de aliniere pentru sintaxa JSX. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // Nerecomandat
    <Foo parametruSuperLung="bar"
         altParametruSuperLung="baz" />

    // RECOMANDAT
    <Foo
      parametruSuperLung="bar"
      altParametruSuperLung="baz"
    />

    // dacă props se potrivesc într-o singură linie, păstrează-l în aceeași linie
    <Foo bar="bar" />

    // copii se indentează normal
    <Foo
      parametruSuperLung="bar"
      altParametruSuperLung="baz"
    >
      <Quux />
    </Foo>
    ```

## Ghilimele

  - Folosește ghilimele duble (`"`) pentru atributele JSX, și ghilimele simple pentru restul JS. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > De ce? atributele JSX [nu pot să conțină ghilimele *escaped*](http://eslint.org/docs/rules/jsx-quotes), ghilimele duble fac o conjuncție pentru `"don't"` mai ușor de scris.
  > Atributele clasice de HTML folosesc ghilimele duble în loc de cele simple, așadar atributele JSX se folosesc de această convenție.

    ```jsx
    // Nerecomandat
    <Foo bar='bar' />

    // RECOMANDAT
    <Foo bar="bar" />

    // Nerecomandat
    <Foo style={{ left: "20px" }} />

    // RECOMANDAT
    <Foo style={{ left: '20px' }} />
    ```

## Spațiere

  - Tot timpul trebuie inclus un singur spațiu într-un tag simplu.

    ```jsx
    // Nerecomandat
    <Foo/>

    // very bad
    <Foo                 />

    // Nerecomandat
    <Foo
     />

    // RECOMANDAT
    <Foo />
    ```

  - Nu scrie JSX folosind acoladele cu spații. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // Nerecomandat
    <Foo bar={ baz } />

    // RECOMANDAT
    <Foo bar={baz} />
    ```

## Props

  - Tot timpul folosește camelCase pentru numele proprietățiilor.

    ```jsx
    // Nerecomandat
    <Foo
      NumeUtilizator="hello"
      numar_telefon={12345678}
    />

    // RECOMANDAT
    <Foo
      numeUtilizator="hello"
      numarTelefon={12345678}
    />
    ```

  - Omite valoarea unei proprietăți când este egală cu `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // Nerecomandat
    <Foo
      ascuns={true}
    />

    // RECOMANDAT
    <Foo
      ascuns
    />
    ```

  - Tot timpul include proprietatea `alt` la tag-urile `<img>`. În cazul în care imaginea este de prezentare, `alt` poate fi un caracter gol sau `<img>`-ul trebuie să conțină `role="prezentare"`. eslint: [`jsx-a11y/img-has-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-has-alt.md)

    ```jsx
    // Nerecomandat
    <img src="salut.jpg" />

    // RECOMANDAT
    <img src="salut.jpg" alt="Eu facand cu mana" />

    // RECOMANDAT
    <img src="salut.jpg" alt="" />

    // RECOMANDAT
    <img src="salut.jpg" role="prezentare" />
    ```

  - Nu folosi cuvinte ca "image", "photo", or "picture" în `<img>` `alt` props. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

  > De ce? Cititoarele de ecran (pentru nevăzători) întotdeauna anunță elementele `img` ca și imagini, așadar nu trebuie să mai conțină cuvintele respectiva în proprietarea alt.

    ```jsx
    // Nerecomandat
    <img src="salut.jpg" alt="Imagine cu mine facand cu mana" />

    // RECOMANDAT
    <img src="salut.jpg" alt="Eu facand cu mana" />
    ```

  - Folosește numai valide, neabstracte [roluri ARIA](https://www.w3.org/TR/wai-aria/roles#role_definitions). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // Nerecomandat - nu este un rol ARIA
    <div role="datepicker" />

    // Nerecomandat - rol abstract ARIA
    <div role="range" />

    // RECOMANDAT
    <div role="button" />
    ```

  - Nu folosi `accessKey` la elemente. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > De ce? Există inconsecvențe între comenzi rapide de la tastatură și comenzile de la tastatură utilizate de către persoanele care folosesc cititoare de ecran (nevăzători) și tastaturi complicate de accesibilitate.

  ```jsx
  // Nerecomandat
  <div accessKey="h" />

  // RECOMANDAT
  <div />
  ```

## Parantezele

  - Încadrează tag-urile JSX în paranteze cand conțin mai mult de o linie. eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```jsx
    // Nerecomandat
    render() {
      return <ComponentaMea className="ceva lung" foo="bar">
               <ComponentaCopil />
             </ComponentaMea>;
    }

    // RECOMANDAT
    render() {
      return (
        <ComponentaMea className="ceva lung" foo="bar">
          <ComponentaCopil />
        </ComponentaMea>
      );
    }

    // RECOMANDAT, cand este o singură linie
    render() {
      const body = <div>salut</div>;
      return <ComponentaMea>{body}</ComponentaMea>;
    }
    ```

## Tag-uri

  - Întotdeauna folosește tag-urile self-close când nu există copii. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // Nerecomandat
    <Foo className="chestii"></Foo>

    // RECOMANDAT
    <Foo className="chestii" />
    ```

  - Dacă componenta are proprietăți multiple, închide tag-ul pe o nouă linie. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // Nerecomandat
    <Foo
      bar="bar"
      baz="baz" />

    // RECOMANDAT
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Metode

  - Folosește funcțiile arrow să incluzi variabile locale.

    ```jsx
    function ListaElemente(props) {
      return (
        <ul>
          {props.elemente.map((element, index) => (
            <Item
              key={element.key}
              onClick={() => faceCevaCu(element.nume, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - Leagă evenimentele de manipulare (onClick, onBlur, onChange) pentru metoda render în constructor. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > De ce? Când o nouă legătura se apelează (cu bind) în render atunci se creează o nouă functie pentru fiecare render.

    ```jsx
    // Nerecomandat
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // RECOMANDAT
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // face chestii
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - NU folosi prefixul underscore pentru metode interne într-o componentă React.

    ```jsx
    // Nerecomandat
    React.createClass({
      _onClickSubmit() {
        // face chestii
      },

      // alte chestii
    });

    // RECOMANDAT
    class extends React.Component {
      onClickSubmit() {
        // face chestii
      }

      // alte chestii
    }
    ```

  - Asigură să returnezi o valoare in metodele `render`. eslint: [`require-render-return`](https://github.com/yannickcr/eslint-plugin-react/pull/502)

    ```jsx
    // Nerecomandat
    render() {
      (<div />);
    }

    // RECOMANDAT
    render() {
      return (<div />);
    }
    ```

## Ordonarea

  - Ordonarea pentru `class extends React.Component`:

  1. opțional methodele `static`
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers sau eventHandlers* ca și `onClickSubmit()` sau `onChangeDescription()`
  1. *metode getter pentru `render`* ca și `getSelectReason()` sau `getFooterContent()`
  1. *metode optionale render* ca și `renderNavigation()` sau `renderProfilePicture()`
  1. `render`

  - Cum definești `propTypes`, `defaultProps`, `contextTypes`, etc...

    ```jsx
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Ziua bună',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Ordonarea pentru `React.createClass`: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers sau eventHandlers* ca și `onClickSubmit()` sau `onChangeDescription()`
  1. *metode getter pentru `render`* ca și `getSelectReason()` sau `getFooterContent()`
  1. *metode optionale render* ca și `renderNavigation()` sau `renderProfilePicture()`
  1. `render`

## `isMounted`

  - NU folosii `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > De ce? [`isMounted` este un anti-model][anti-model], nu este disponibil pentru folosirea claselor ES6, și este pe punctul de a fi oficial exclusă.

  [anti-model]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[⬆ înapoi sus](#cuprins)**
