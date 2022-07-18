# Trabajo: Creación de una extensión para Visual Studio Code

_Acá va un párrafo que describa lo que es el proyecto_

## Objetivo 🚀

_Practicar JavaSctript y de paso conocer TypeScript y aplicarlo de una forma novedosa y de una forma usada hoy en dia._

## Descripcion
Visual Studio Code se esta convirtiendo en un editor de codigo muy bueno gracias a su cantidad de **Extenciones**. En este caso se aplicara la creacion de una extencion usando TypeScript y algunas Librerias de NodeJS.


## Pre-requisitos 📋

_Que cosas necesitas para instalar el software y como instalarlas._


*   **Visual Studio Code** // Es el editor de codigo del cual anteriormente se hablo.
*   **NodeJS** // Se usara para descargar paquetes de sus repositorios como lo son:
    *   **Yeoman** : Es una aplicacion para generar el codigo del esqueleto inicial.   
    *   **generator-code** : Es uno de los generadores para Yeoman y nos servira para comenzar el desarrollo de la extencion para VS Code.

## Instalación & Iniciación🔧
_Podemos Instalar todo esto desde la terminal:_
``` terminal
npm install -g yo generator-code typescript
```
_Una vez instalado ejecutar en terminal:_
```
yo code
 ```
 _Aqui Seleccionamos New Extension (TypeScript)._
 ![Sourcefile](https://i.postimg.cc/fTVRfpd0/Yoco.png "Aqui Seleccionamos New Extension (TypeScript)")
 _Luego Agregar nombre, nombre de extencion, descripcion, etc..._
 ![SF2](https://i.postimg.cc/gk1kSsyt/yoco2.png "")
_Y por ultimo se nos abrira nuestro VS Code con el projecto de extención._
![SF3](https://i.postimg.cc/0ym5LSck/yoco3.png)



## Editando el codigo ✍

_Nuestro Codigo a Modificar(Codigo de ejemplo) lo encontraremos en la carpeta de **src/extencion.ts**._

_por otro lado las propiedades de la extencion las modificaremos en el archivo **package.json** en la raiz del projecto._

---
_En el archivo **package.json** se editara lo siguiente_
```json
{
    "name": "gapline", //Aqui se encuentra el nombre de la Extencion
    "displayName": "Line Gapper", //Este es el nombre del projecto
    "description": "Esta extencion es un ejemplo de la asignatura de clinte servider en la tarea de crear una extencion para VSCode", //Aqui se encuentra la descripcion del projecto
    "version": "0.0.1",
    "engines": {
        "vscode": "^1.67.0"
    },
    "categories": [
        "Other"
    ],
    "activationEvents": [
        "onCommand:extension.gapline" // aqui llamaremos a la extencion para que no cargue tanto en memoria solo cuando se ocupe
    ],
    "main": "./out/extension.js",
    "contributes": {
        "commands": [{  //Aqui se llamaran a los comandos y el nombre de cada uno.
            "command": "extension.gapline", //Con este usaremos la funcion
            "title": "Gapeline" //Asi es como lo encontraremos en la consola de proyectos
        }]
    },
    "scripts": {
        "vscode:prepublish": "npm run compile",
        "compile": "tsc -p ./",
        "watch": "tsc -watch -p ./",
        "pretest": "npm run compile && npm run lint",
        "lint": "eslint src --ext ts",
        "test": "node ./out/test/runTest.js"
    },
    "devDependencies": {
        "@types/vscode": "^1.67.0",
        "@types/glob": "^7.2.0",
        "@types/mocha": "^9.1.1",
        "@types/node": "14.x",
        "@typescript-eslint/eslint-plugin": "^5.21.0",
        "@typescript-eslint/parser": "^5.21.0",
        "eslint": "^8.14.0",
        "glob": "^8.0.1",
        "mocha": "^9.2.2",
        "typescript": "^4.6.4",
        "@vscode/test-electron": "^2.1.3"
    }
}
```
Por otro lado esta es la Extenvion Gapper Line.

```TypeScript
'use strict'; // Invoca al modo estricto
import * as vscode from 'vscode'; //Importa el modulo de VSCODE
export function activate(context: vscode.ExtensionContext) {	//Crea la Funcion para la extencion y la retorna publica para otras api
    let disposable = vscode.commands.registerCommand('extension.gapline', () => { //Crea el comando para ejecutar
        var editor = vscode.window.activeTextEditor; //esta guardando el editor de texto y lo guarda en la variable editor
        if (!editor) { return; } //Valida la funcion editor
        var selection = editor.selection; //Toma todo lo que esta presente en el GUI
        var text = editor.document.getText(selection); //Toma el texto seleccionado y lo almacena en la variable text	
        vscode.window.showInputBox({ prompt: 'Lineas?' }).then(value => { //Abre un recuadro donde le pregunta al usuario que introdusca
            let numberOfLines = +value; //Toma el numero de lineas
            var textInChunks: Array<string> = [];//Crea una var para almacenar el arreglo
            text.split('\n').forEach((currentLine: string, lineIndex) => { //separa el texto con espacios
                textInChunks.push(currentLine); //Manda el texto a determinada posicion
                if ((lineIndex+1) % numberOfLines === 0) {textInChunks.push('');} //Valida el push de las lineas mandadas
            });
            text = textInChunks.join('\n'); // Agrega todos los elementos a la cadena
			editor.edit((editBuilder) => { // Asocia el editor de texto
                var range = new vscode.Range( //crea el rango del editor de codigo
                    selection.start.line, 0,  //de donde va empezar el rango
                    selection.end.line, //donde termina el rango
                    editor.document.lineAt(selection.end.line).text.length //alinea el texto
                );
                editBuilder.replace(range, text); //Centro el nuevo texto con el nuevo rango
            });
        });
    });
    context.subscriptions.push(disposable);//deside si el contenido se puede añadir 
}
export function deactivate() { } //sirve para realizar una limpieza
```
## Ejecutando las pruebas ⚙️

_Para ejecutar la extencion es necesario precionar la Tecla **F5** el cual nos abrira otra ventana de VS Code_
![SCF1](https://i.postimg.cc/JnLFJ3M2/f51.png "Al precionar el F5 se despliega otra ventana de visual")
_Una vez que se nos abrio la ventana nueva crear un documento de texto plano._
![SCF2](https://i.postimg.cc/R0F5hLCQ/nf1.png "Aqui se abrio el doc de texto plano")
_Despues precionar las teclas **Ctrl + Mayus + P** el cual nos abrira un TextBox en el cual se pone el nombre de la extención._
![SCF3](https://i.postimg.cc/sx5bqnKy/nf2.png "Aqui ya aparece la ComboBox")
_Una vez ya seleccionada la extencion te pedira el numero de lineas._
![SCF4](https://i.postimg.cc/ydgMX6ZD/nf3.png "Aqui se puso como ejemplo #1")


### Y las pruebas de estilo de codificación ⌨️

_Una vez dado enter remplazara las lines por bloques en blanco._
![SCF5](https://i.postimg.cc/C13WvyGm/nf4.png "Aqui esta el resultado")

## Despliegue 📦

_En caso de que el programa no funcione verificar el archivo ***package.json***  y verificar nombres y comandos esten de forma correcta y que correspondan con la funcion descrita en el TS_

## Construido con 🛠️

_Menciona las herramientas que utilizaste para crear tu proyecto_

* [Visual Studio Code](https://code.visualstudio.com/) - El IDE Usado
* [Documentacion de Extenciones VS Code](https://code.visualstudio.com/docs/editor/extension-gallery) - Encuentras la documentacion de extenciones
* [TypeScript](http://www.typescriptlang.org/) - Lenguaje basado en JavaScript de Microsoft
* [NodeJS](https://nodejs.org/en/download/current/) - Motor de JavaScript
* [Yeoman](https://yeoman.io/) - Aplicacion para generar el esqueleto de un proyecto web
* [Generator-Code](https://www.npmjs.com/package/generator-code) - Es uno de los generadores para Yeoman y sirve para comenzar el desarrollo de una extencion de VS Code


## Licencia 📄

Este practica fue proporcionada por la Universidad de la Rioja para la materia de Cliente Servidor.



---
⌨️ con ❤️ por [Luisgr95](https://github.com/luisgr95) 😊
