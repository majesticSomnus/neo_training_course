# Homework

## HW1

Finish Domain Smart Contract

```csharp
private static bool Delete(string domain)
{
    byte[] owner = Storage.Get(Storage.CurrentContext, domain);
    if (owner == null) return false;

    if (!Runtime.CheckWitness(owner)) return false;

    Storage.Delete(Storage.CurrentContext, domain);
    return true;
}
```

## HW2

Create a new NEP5 Smart Contract

[Code](./HZC.cs)

![Result](./HZC.png)

## HW3

Query NNC balance of ATgjfbkkgAgpfGe1DiKjLwvGSXZ7MMUjZU

neon-js Code 
```js
var neon = require("@cityofzion/neon-js");

client = new neon.rpc.RPCClient("http://seed2.aphelion-neo.com:10332");

nnc = 'fc732edee1efdf968c23c20a9628eaa5a6ccb934';

query = neon.default.create.query({ method: "getnep5balances", params: ['ATgjfbkkgAgpfGe1DiKjLwvGSXZ7MMUjZU'] });
client.execute(query).then(res => {

    for (var b of res.result.balance) {
        if (b.asset_hash == nnc) {
            console.log(b.amount);
        }
    }
});
```
Result

![res](./balance-js.jpg)

Go Code
```go
package main

import (
	"fmt"
	"github.com/hzxiao/goutil"
	"github.com/hzxiao/goutil/httputil"
)

var seed = "http://seed2.aphelion-neo.com:10332"
var nnc = "fc732edee1efdf968c23c20a9628eaa5a6ccb934"

func main()  {
	reqArgs := goutil.Map{
		"id": 1,
		"jsonrpc": "2.0",
		"method": "getnep5balances",
		"params": []interface{}{"ATgjfbkkgAgpfGe1DiKjLwvGSXZ7MMUjZU"},
	}

	var res goutil.Map
	err := httputil.PostJSON(seed, reqArgs, httputil.ReturnJSON, &res)
	if err != nil {
		fmt.Println(err)
		return
	}

	for _, b := range res.GetMapArrayP("result/balance") {
		if b.GetString("asset_hash") == nnc {
			fmt.Printf("balance: %v\n", b.GetInt64("amount"))
		}
	}
}
```

Ruselt

![Res](./balance.png)