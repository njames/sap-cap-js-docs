---
synopsis: >
  Looking for the obligatory greeting? &rarr; here you go.
notebook: true
notebooklanguages: node, java
notebooktitle: Hello World
status: released
impl-variants: true
---

# Hello World!

Let's create a simple  _Hello World_ OData service using the SAP Cloud Application Programming Model in six lines of code and in under 2 minutes. {.impl .node}

You can also download the [sample from github.com](https://github.com/sap-samples/cloud-cap-samples/tree/main/hello). {.impl .node}

Let's create a simple _Hello World_ OData service using the SAP Cloud Application Programming Model with a few lines of code and in under 2 minutes. {.impl .java}

<!--- % include _toc %} -->
## Initialize the project {.impl .java}

<div class="impl java">

```sh
cds init hello-world --add java,samples
cd hello-world
```
</div>

## Define a Service
... using [CDS](../cds/).

File _srv/world.cds_, content:
```cds
service say {
  function hello (to:String) returns String;
}
```



## Implement it

... for example, using [Node.js](../node.js/) express.js handlers style. {.impl .node}

File _srv/world.js_, content: {.impl .node}

<div class="impl node">

```js
module.exports = (say)=>{
  say.on ('hello', req => `Hello ${req.data.to}!`)
}
```

</div>

... or [Node.js](../node.js/) ES6 classes style. {.impl .node}


File _srv/world.js_, content: {.impl .node}

<div class="impl node">

```js
module.exports = class say {
  hello(req) { return `Hello ${req.data.to}!` }
}
```
</div>

> That has limited flexibility, for example, you can register only one handler per event. { .impl .node}

... for example, using a [CAP Java](../java/provisioning-api) custom handler like this:

File _srv/src/main/java/customer/hello_world_java/handlers/HelloWorld.java_, content:

```java
package customer.hello_world_java.handlers;

import org.springframework.stereotype.Component;

import com.sap.cds.services.handler.EventHandler;
import com.sap.cds.services.handler.annotations.On;
import com.sap.cds.services.handler.annotations.ServiceName;

import cds.gen.say.HelloContext;
import cds.gen.say.Say_;

@Component
@ServiceName(Say_.CDS_NAME)
public class HelloHandler implements EventHandler {

	@On
	public void onHello(HelloContext ctx) {
		ctx.setResult("Hello " + ctx.getTo());
		ctx.setCompleted();
	}

}
```

## Run it
... for example, from your command line in the root directory of your "Hello World":

<div class="impl node">

```sh
cds watch
```
</div>

<div class="impl java">

```sh
cd srv
mvn cds:watch
```
</div>


## Consume it
... for example, from your browser:<br>

<http://localhost:4004/say/hello(to='world')>  { .impl .node}

<!-- <http://localhost:4004/say/hello?to=world> -->
http://localhost:8080/odata/v4/say/hello(to='world') { .impl .java}

<!--- % include links.md %} -->