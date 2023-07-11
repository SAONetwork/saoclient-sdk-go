# Get sao-client-go sdk

add sao-client-go dependency to your project:

```
$ go get github.com/SaoNetwork/sao-client-go
```

# Prerequisite

follow sao client cmd tutorial to prepare local sao did client.

# Usage

Prepare local sao did.

* import sao-client-go package

```
import "github.com/SaoNetwork/sao-client-go/sdk"
```

* initialize sao client

  ```
  nodeUrl := "http://127.0.0.1:5151/rpc/v0"
  chainUrl := "http://127.0.0.1:26657"
  didKey := "<didkey>"
  client, err := sdk.NewSaoClientApi(ctx, nodeUrl, didKey)
  ```

node url is endpoint to connect to gateway.

chain url is rpc endpoint to chain.

did key is local did client's key.

#### Create Model

```
groupId := "your application name"
duration := uint64(365)
delay := uint64(10)
name := "my profile"
replicas := 5
content := "your data model"
alias, dataId, err := client.CreateModel(ctx, content, groupId, duration, delay, name, replicas)

```

#### Show Commits

```
resp, err := client.ShowCommits(ctx, dataId, groupId)
if err != nil {
	// handle error
	return
}
fmt.Println("alias: ", resp.Alias)
fmt.Println("dataId: ", resp.DataId)
fmt.Println("Commits: ")
for _, commit := range resp.Commits {
	fmt.Println(commit)
}
```

#### Load Model

```
fmt.Println("load model...")
bytes, err := client.Load(ctx, dataId, "", , groupId)
if err != nil {
	// handle error
	return
}
fmt.Println("load model: ", string(bytes))
```

#### Update Permission

```
err = client.UpdatePermission(ctx, dataId,
	[]string{"did:key:zQ3shggYEtCZNEiwSeqLdLo97SqS2ERMHB2mgV8hmCGDn4DJ3"},		    		    	[]string{"did:key:zQ3shggYEtCZNEiwSeqLdLo97SqS2ERMHB2mgV8hmCGDn4DJ3"},
)
if err != nil {
    // handle error
	return
}
fmt.Println("Permission Updated.")
```

#### Update Model

First step is to generate change patch

```
origin := "{\"nickname\":\"old name\"}"
target := "{\"nickname\":\"new name\"}"
patch, cid, contentLen, err := client.PatchGen(origin, target)
if err != nil {
	// handle error
	return
}
```

Then update data model

```
alias, dataId, commitId, err := client.UpdateModel(ctx, patch, duration, delay, force, keyword, commitId, cid, size, replicas, groupId)
if err != nil {
	// handle error
	return
}
```

