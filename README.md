# koa-contract
a interface definition lib for koa

# summary
Use a js Object to give web api definition including url, method, description, input parameters definition and result definition. The object is called as Contract;

Then you can specifiy which Contracts to be loaded by node-contract framework.

# parameter checking
When request comes, framework will check the actual parameters against defined parameters. if any problem happens, like require parameter is not found or type mismatch, framework will terminate the further process and return error.

# sample contract definition
```javascript
var Contract = require('node-contract').Contract
var contract = Contract({
  name: 'test api',
  url: '/test/api',
  method: 'get',
  description: 'a test api',
  params: {
    type: { TYPE: String, from: 'query', description: 'type' },
    page: { TYPE: Number, from: 'query', require: false, description: 'which page' },
    pageSize: { TYPE: Number, from: 'query', require: false, description: 'size of page ' },
    platform: { TYPE: String, from: 'body', in: ['ios', 'android'] }
  },
  result: { TYPE: [{name: String}], description: 'result is a list of object, each contains name fields' },
  processor: getList
}
//this is processor function which take params and output result
function* getList(type, page, pageSize) {
  return [{
    name: 'test1'
  }, {
    name: 'test2'
  }];
}
```
the above code give a clear definition of web api. Also it restricts the params for us.
type is a required String param and comes from querystring
page is a optional Number param and also comes from querystring
pageSize is a optional Number param and also comes from querystring

# integrate with KOA
use built-in bridge to connect KOA and Contract
```javascript
var KoaBridge = require('node-contract').KoaBridge
//choose which contracts to use
var bridge = KoaBridge({ contracts: contracts });
//or KoaBridage('folder path'); //load all contracts from js file under the path

//mount function convert all loaded contract to koa router
koaApp.use(bridge.mount());//load all loaded contracts
//or koaApp.use(bridge.mount([{ name:'C1'}, { name: 'C2'}])); //load partial contracts by name
```

# integrate with KOA2 or express
on the way...