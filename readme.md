# azure-blob-list-stream [![tests](https://github.com/bendrucker/azure-blob-list-stream/workflows/tests/badge.svg)](https://github.com/bendrucker/azure-blob-list-stream/actions?query=workflow%3Atests)

> Stream a list of all files from an Azure Blob Storage collection

## Deprecated

This package is deprecated and no longer maintained. It depends on the legacy [`azure-storage`](https://www.npmjs.com/package/azure-storage) SDK, which Microsoft retired. Use [`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob) instead. Its `listBlobsFlat().byPage()` iterator covers this package's job, including page events and resumable continuation tokens:

```js
const { ContainerClient } = require('@azure/storage-blob')

const container = new ContainerClient(connection, 'my-container')

for await (const page of container.listBlobsFlat().byPage({ continuationToken: token })) {
  for (const blob of page.segment.blobItems) {
    console.log(blob.name)
  }
}
```

## Install

```
$ npm install --save azure-blob-list-stream
```


## Usage

```js
const ListStream = require('azure-blob-list-stream')
const blob = azure.createBlobService(process.env.AZURE_STORAGE_CONNECTION_STRING)

ListStream(blob, 'my-container')
  .on('data', (blob) => console.log(blob.name))
```

The stream respects backpressure and will only read in new pages from Azure when its internal buffer is empty.

## API

#### `ListStream(blob, container, token)` -> `Readable`

Returns a readable stream that emits each Blob Storage file entry.

##### blob

*Required*  
Type: `object`

An Azure Blob Storage interface that you construct.

##### container

*Required*  
Type: `string`

The name of the storage container to list.

##### token

Type: `string`  
Default: `undefined`

A page token at which to start the list operation. 

#### `stream.on('page', handler)`

Listens on the `page` event which is emitted each time a new page of results arrives.

##### handler

*Required*  
Type: `function`  
Arguments: `{token, count}`

## License

MIT © [Ben Drucker](http://bendrucker.me)
