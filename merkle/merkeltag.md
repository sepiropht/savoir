# Merkle Tag

## Why

Essentialy hashes of evenly grouped chunk of data (anything: files, audio, video, strings). Useful to verify some part of data without need to have all the data.
used in distributed network like bitcoin or dat.

#### Basic implementation

```javascript
var crypto = require("crypto");
// function that regroup data evenly
//
function group_hashes(file_hashes) {
  var groups = [];
  var remaining = file_hashes.splice(2);

  while (file_hashes.length > 0) {
    var c = crypto.createHash("sha256");
    c.update(file_hashes[0] + file_hashes[1]);

    groups.push(c.digest("hex"));

    file_hashes = remaining;
    remaining = file_hashes.splice(2);
  }
  return groups;
}

function merkel_hash(file_hashes) {
  var len = file_hashes.length;
  while (len % 2 !== 0) {
    file_hashes.push(file_hashes[len - 1]);
    len = file_hashes.length;
  }

  file_hashes = group_hashes(file_hashes);

  if (file_hashes.length === 1) {
    return file_hashes.pop();
  } else {
    return merkel_hash(file_hashes);
  }
}

var n = merkel_hash(["ssfsfsf", "tdfsfsdf", "rssff", "dfdf", "fdldskjfkjsdmf"]);

console.log("the merkel tag is: ", n);
```
## See Also
- [video sharing files](https://www.youtube.com/watch?v=hiBalbJ2I2I )
