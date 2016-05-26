
[![redux-i18n in Travis](https://travis-ci.org/APSL/redux-i18n.svg?branch=master)](https://travis-ci.org/APSL/redux-i18n)

# Description

**redux-i18n** is a simple and powerful package for translate your *react* application with *react-redux*.

## Install

```
npm i redux-i18n --save
```

## How to use

This package provide a parent component that encapsulate your application. See this example for understand it.

```javascript
import I18n from "redux-i18n"
import {translations} from "./translations"

class MainApp extends React.Component {
  render() {
    return (
      <I18n translations={translations}>
        <div>
            <h1>My Project</h1>
            {this.props.children}
        </div>
      </I18n>
    )
  }
}
```

Where *translations* is a dictionary as this:

```javascript
export const translations = {
  es: {
    "Translate this text": "Traduce este texto",
    "Hello {n}!": "Hola {n}!",
  }
}
```

## Setting the reducer

In your *combineReducers* you should add **i18nState** reducer.

```javascript
import {otherreducers} from "./Yourproject"
import {i18nState} from "redux-i18n"

const appReducer = combineReducers({
    otherreducers,
    i18nState
})
```

Now, you can connect *lang* attribute in your component:

```javascript
export default connect(state => ({
  lang: state.i18nState.lang,
}))(Home)
```

## Translating the content

You can access to a *I18n's* functions with your component's context. For example:

```javascript
Home.contextTypes = {
  t: React.PropTypes.func.isRequired
}
```

And then, you can use *i18n* function in your render method.

```javascript
render() {
    return (
      <div>
        <strong>Your current language, is: {this.props.lang}</strong><br/>
        {this.context.t("Translate this text")}<br/>
        {this.context.t("Hello {n}!", {n: "Cesc"})}<br/><br/>
        <button onClick={this.changeLanguage.bind(this)}>Change Language</button>
      </div>
    )
}
```

## Changing the language

This library has a *setLanguage* action. You can use as follow:

```javascript
import {setLanguage} from "redux-i18n"

componentWillMount() {
    this.props.dispatch(setLanguage("es"))
}
```

## Using the scripts

This package have two scripts for extract texts to a *template.pot* and import texts from *po* file to a *translation.js* file.

For use this scripts, you should edit your *package.json* and add both scripts.

```json
    "scripts": {
        "extract": "i18n_extract",
        "import": "i18n_import"
    }    
```

In this example I have used *extract* and *import* name for scripts, but you can use any. For run one of any scripts:

```
npm run extract
npm run import
```

## Extract texts and generate template.pot

```
npm run extract
```

This script search all results inside **src** folder with a regular expression and generate a **locales/template.pot** file. This file can be used with [Poedit](https://poedit.net/) application for generate *en.po*, *es.po*, etc. files.

You should save your *po* files inside *locales* folder.

If you want set other source folder, you can use *source* parameter.

```json
    "scripts": {
        "extract": "i18n_extract --source=mysource",
        "import": "i18n_import"
    }    
```

Or if you want set other *locales* folder:

```json
    "scripts": {
        "extract": "i18n_extract --source=mysource --locales=mylocales",
        "import": "i18n_import"
    }    
```

## Import your po files

```
npm run import
```

This script load all po files inside you *locales* folder, extract all translations and build a **src/translations.js** that you will use in your application.

You can set other *locales* folder:

```json
    "scripts": {
        "extract": "i18n_extract --source=mysource --locales=mylocales",
        "import": "i18n_import --locales=mylocales"
    }    
```

If you want save *translation.js* file to other folder:

```json
    "scripts": {
        "extract": "i18n_extract --source=mysource --locales=mylocales",
        "import": "i18n_import --locales=mylocales --translations=myfolder"
    }    
```
