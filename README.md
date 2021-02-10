# Filesystem


## webkitRequestFileSystem

```javascript
window.webkitRequestFileSystem(
      // Whether the storage should be persistent. Possible values are TEMPORARY or PERSISTENT.
      //  Data stored using TEMPORARY can be removed at the browser’s discretion (for example if more space is needed)
      //  PERSISTENT storage cannot cleared unless explicitly authorized by the user or the application.
      Window.PERSISTENT,
      // An indicator of how much storage space, in bytes, the application expects to need.
      1024 * 1024 * 5,
      // A callback function that is called when the user agent successfully provides a filesystem. Its argument is a FileSystem object.
      onInitFs,
      // An optional callback function which is called when an error occurs, or the request for a filesystem is denied. Its argument is a FileError object.
      errorHandler
    );
```

## errorHandler
```javascript
function errorHandler(Erro) {
  if (Erro.name === 'QuotaExceededError') {
    window.webkitStorageInfo.requestQuota(
      window.PERSISTENT,
      1024 * 1024 * 5,
      function (bytes) {
        alert("Quota is available: " + bytes);
      },
      function (e) {
        alert("Error allocating quota: " + e);
      }

    );
  }


  console.log({ Erro });
}
```
## onInitFs
```javascript
function onInitFs(fs, settext) {
   console.log('Opened file system: ' + fs.name);
  }
```
## FileReader 
```javascript
// Obtain the File object representing the FileEntry.
// Use FileReader to read its contents.
fs.root.getFile('log.txt', {
    // create: true, 
    exclusive: true
},
    function (fileEntry) {
        fileEntry.file(function (file) {
            var reader = new FileReader();

            reader.onloadend = function (e) {
                console.log(this.result);
            };

            reader.readAsText(file); // Read the file as plaintext.
        }, errorHandler);
    },
    errorHandler
);
```

## createWriter
```javascript
// https://developers.google.com/web/updates/2012/06/Don-t-Build-Blobs-Construct-Them
fs.root.getFile('log.txt', {
    // create: true, 
    exclusive: true
},
    function (fileEntry) {
        fileEntry.createWriter(function (fileWriter) {

            fileWriter.onwrite = function (e) {
                console.log('Write completed.');
            };

            fileWriter.onerror = function (e) {
                console.log('Write failed: ' + e.toString());
            };
            const bb = new Blob(['Lorem Ipsum']);

            fileWriter.write(bb);

        }, errorHandler);
    },
    errorHandler
);

```

## License
[MIT](https://choosealicense.com/licenses/mit/)
