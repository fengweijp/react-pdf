![downloads](https://img.shields.io/npm/dt/react-pdf.svg) ![build](https://img.shields.io/travis/wojtekmaj/react-pdf.svg) ![dependencies](https://img.shields.io/david/wojtekmaj/react-pdf.svg
) ![dev dependencies](https://img.shields.io/david/dev/wojtekmaj/react-pdf.svg
)

# React-PDF
Easily display PDF files in your React application.

## tl;dr
* Install by executing `npm install --save react-pdf`.
* Import by addding `import { Document } from 'react-pdf'`.
* Use by adding `<Document file="..." />`. `file` can be an URL, base64 content, Uint8Array, and more.
* Put `<Page />` components inside `<Document />` to render pages.

## Demo

Minimal demo page is included in sample directory.

[Online demo](http://projekty.wojtekmaj.pl/react-pdf/) is also available!

## Getting started

### Prerequisites

You'll need to have Node >= 4 on your machine.

We strongly recommend to use Node >= 6 and npm >= 5 for faster installation speed and better disk usage.

#### Compatibility

Your project needs to use React 15.5 or later. If you use older version of React, please refer to the table below to find suitable React-PDF version.

|React version|Newest supported React-PDF|
|----|----|
|>15.5|latest|
|>15.0|1.6.1|
|>0.14|0.0.10|
|>0.13|0.0.10|
|>0.11|0.0.4|

### Installation

Add React-PDF to your project by executing `npm install --save react-pdf`.

### Usage

Here's an example of basic usage:

```js
import React, { Component } from 'react';
import { Document, Page } from 'react-pdf';

class MyApp extends Component {
  state = {
    numPages: null,
    pageNumber: 1,
  }

  onDocumentLoad({ numPages }) {
    this.setState({ numPages });
  }

  render() {
    return (
      const { pageNumber, numPages } = this.state;

      <div>
        <Document
          file="somefile.pdf"
          onLoadSuccess={this.onDocumentLoad}
        >
          <Page pageNumber={pageNumber} />
        </Document>
        <p>Page {pageNumber} of {numPages}</p>
      </div>
    );
  }
}
```

Check the sample directory of this repository for a full working example.

### Enable PDF.js worker

It is crucial for performance to use PDF.js worker whenever possible. This ensures that your PDF file will be rendered in a separate thread without affecting page performance. To make things a little easier, we've prepared several entry points you can use.

#### Webpack

If you use Webpack, you're in luck. Instead of directly importing/requiring `'react-pdf'`, import it like so:

```js
import { Document } from 'react-pdf/build/entry.webpack';
```

…and you're all set!

#### Browserify and others

If you use Browserify or other bundling tools, you will have to make sure on your own that `pdf.worker.js` file from `pdfjs-dist/build` is copied to your project's output folder.

#### I give up

If you absolutely have to, you can import React PDF with worker disabled. You can do so by importing React-PDF like so:

```js
import { Document } from 'react-pdf/build/entry.noworker';
```

## User guide

### Document

Loads a document passed using `file` prop.

#### Props

|Prop name|Description|Example values|
|----|----|----|
|className|Defines custom class name(s), that will be added to rendered element along with the default `ReactPDF__Document`.|<ul><li>String:<br />`"custom-class-name-1 custom-class-name-2"`</li><li>Array of strings:<br />`["custom-class-name-1", "custom-class-name-2"]`</li></ul>|
|error|Defines what the component should display in case of an error. Defaults to "Failed to load PDF file.".|<ul><li>String:<br />`"An error occurred!"`</li><li>React element:<Br />`<div>An error occurred!</div>`</li><li>Function:<Br />`this.renderError()`</li></ul>|
|file|Defines what PDF should be displayed.<br />Its value can be an URL, a file (imported using `import ... from ...` or from file input form element), or an object with parameters (`url` - URL; `data` - data, preferably Uint8Array; `range` - PDFDataRangeTransport; `httpHeaders` - custom request headers, e.g. for authorization), `withCredentials` - a boolean to indicate whether or not to include cookies in the request (defaults to `false`).|<ul><li>URL:<br />`"http://example.com/sample.pdf"`</li><li>File:<br />`import sample from '../static/sample.pdf'` and then<br />`sample`</li><li>Parameter object:<br />`{ url: 'http://example.com/sample.pdf', httpHeaders: { 'X-CustomHeader': '40359820958024350238508234' }, withCredentials: true }`</ul>|
|loading|Defines what the component should display while loading. Defaults to "Loading PDF…".|<ul><li>String:<br />`"Please wait!"`</li><li>React element:<Br />`<div>Please wait!</div>`</li><li>Function:<Br />`this.renderLoader()`</li></ul>|
|noData|Defines what the component should display in case of no data. Defaults to "No PDF file specified.".|<ul><li>String:<br />`"Please select a file."`</li><li>React element:<Br />`<div>Please select a file.</div>`</li><li>Function:<Br />`this.renderNoData()`</li></ul>|
|onLoadError|Function called in case of an error while loading a document.|`(error) => alert('Error while loading document! ' + error.message)`|
|onLoadSuccess|Function called when the document is successfully loaded.|`(pdf) => alert('Loaded a file with ' + pdf.numPages + ' pages!')`|
|onSourceError|Function called in case of an error while retrieving document source from `file` prop.|`(error) => alert('Error while retreiving document source! ' + error.message)`|
|onSourceSuccess|Function called when document source is successfully retreived from `file` prop.|`() => alert('Document source retreived!')`|
|rotate|Defines the rotation of the document in degrees. If provided, will change rotation globally, even for the pages which were given `rotate` prop of their own. 90 = rotated to the right, 180 = upside down, 270 = rotated to the left.|`90`|

### Page

Displays a page. Must be placed inside `<Document />` or have `pdf` prop passed, which can be obtained from `<Document />`'s `onLoadSuccess` callback function.

#### Props

|Prop name|Description|Example values|
|----|----|----|
|className|Defines custom class name(s), that will be added to rendered element along with the default `ReactPDF__Page`.|<ul><li>String:<br />`"custom-class-name-1 custom-class-name-2"`</li><li>Array of strings:<br />`["custom-class-name-1", "custom-class-name-2"]`</li></ul>|
|onLoadError|Function called in case of an error while loading the page.|`(error) => alert('Error while loading page! ' + error.message)`|
|onLoadSuccess|Function called when the page is successfully loaded.|`(page) => alert('Now displaying a page number ' + page.pageNumber + '!')`|
|onRenderError|Function called in case of an error while rendering the page.|`(error) => alert('Error while loading page! ' + error.message)`|
|onRenderSuccess|Function called when the page is successfully rendered on the screen.|`() => alert('Rendered the page!')`|
|pageIndex|Defines which page from PDF file should be displayed. Defaults to 0.|`0`|
|pageNumber|Defines which page from PDF file should be displayed. If provided, `pageIndex` prop will be ignored. Defaults to 1.|`1`|
|renderTextLayer|Defines whether a text layer should be rendered. Defaults to true.|`false`|
|rotate|Defines the rotation of the page in degrees. 90 = rotated to the right, 180 = upside down, 270 = rotated to the left. Defaults to page's default setting, usually 0.|`90`|
|scale|Defines the scale in which PDF file should be rendered. Defaults to 1.0.|`0.5`|
|width|Defines the width of the page. If not defined, canvas will be rendered at the width defined in PDF. If you define `width` and `scale` at the same time, the width will be multiplied by a given factor.|`300`|

### Outline

Displays an outline (table of contents). Must be placed inside `<Document />` or have `pdf` prop passed, which can be obtained from `<Document />`'s `onLoadSuccess` callback function.

#### Props

|Prop name|Description|Example values|
|----|----|----|
|className|Defines custom class name(s), that will be added to rendered element along with the default `ReactPDF__Outline`.|<ul><li>String:<br />`"custom-class-name-1 custom-class-name-2"`</li><li>Array of strings:<br />`["custom-class-name-1", "custom-class-name-2"]`</li></ul>|
|onItemClick|Function called when an item has been clicked.|`({ pageNumber }) => alert('Clicked an item from page ' + pageNumber + '!')`|
|onLoadError|Function called in case of an error while retreiving the outline.|`(error) => alert('Error while retreiving the outline! ' + error.message)`|
|onLoadSuccess|Function called when the outline is successfully retreived.|`() => alert('The outline has been successfully retreived.')`|
|onParseError|Function called in case of an error while parsing the outline.|`(error) => alert('Error while parsing the outline! ' + error.message)`|
|onParseSuccess|Function called when the outline is successfully parsed.|`({ outline }) => alert('There are ' + outline.length + ' top level items in the table of contents.')`|

## License

The MIT License.

## Author

<table>
  <tr>
    <td>
      <img src="https://github.com/wojtekmaj.png?s=100" width="100">
    </td>
    <td>
      Wojciech Maj<br />
      <a href="mailto:kontakt@wojtekmaj.pl">kontakt@wojtekmaj.pl</a><br />
      <a href="http://wojtekmaj.pl">http://wojtekmaj.pl</a>
    </td>
  </tr>
</table>

## Thank you

This project wouldn't be possible without awesome work of Niklas Närhinen <niklas@narhinen.net> who created its initial version and without Mozilla, author of [pdf.js](http://mozilla.github.io/pdf.js). Thank you!
