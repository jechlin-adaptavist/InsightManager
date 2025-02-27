# InsightManager

## Background
InsightManager (IM) is a wrapper for the most frequently used Insight JAV API´s, it´s intended to be imported in to your own scripts or apps to get you quicker up and running with the Insight APIs. On top of being a wrapper it also adds additional functionality that is not present in Insights native JAVA API.

IM is built to be as user friendly and flexible as possible. If you are comfortable with the general structure of Insights and know how to work with Groovy Arrays, Maps, Strings and Integers then you will be up and running in no time at all!

IM can be used anywhere ScriptRunner executes groovy scripts, for example workflow transitions, behaviors and REST endpoints. The second release of IM will also support being run by Insight own groovy capabilities.

InsightManager was initially created by Riada AB which at the time was a partner to Mindville who are the creators of Insight. 
InsightManager is still maintained by Riada AB but welcomes any and all inputs and pull requests. 
InsightManager is provided “as is” without warranty of any kind, it is to be used at your own risk and always tested in a non production environment first.

“THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.”

### Update

As of ScriptRunner version 7.11, ScriptRunner has native support for common Insight use-cases, such as creating, retrieving and updating object beans, searching using IQL/AQL, and working with comments and attachments. We call this HAPI.

When you use HAPI your scripts are much simpler and more readable, and avoids you having to add a library to the classpath. 

Read more about working with Insight with HAPI [here](https://docs.adaptavist.com/sr4js/latest/hapi/work-with-assets-insight).

There are a few things this library does that are not covered by HAPI, so you may prefer to use this library. Feel free to let us know about anything missing in HAPI at our [support portal](https://productsupport.adaptavist.com/servicedesk/customer/portal/2).

## Some quick examples to grab your attention
```Groovy
import customRiadaLibraries.insightmanager.InsightManagerForScriptrunner
InsightManagerForScriptrunner im = new InsightManagerForScriptrunner()

//Import all new Departments from AD (Import ID 9)
im.runImport(9, ["Departments"]) 

//Find the department object we are interested in
ObjectBean departmentObject = im.iql(1, "ObjectType = Department AND Name = IT").first() 

//Created a new Employee (ObjectTypeId 5) with a map containing attributes
ObjectBean newEmployee = im.createObject(5, 
		[
			Name: "Nisse Hult", 
			JiraUser : "nisse.hult", 
			Department: departmentObject 
		]
	)

//Update the department object with the new employee as department head and set a status
im.updateObjectAttributes(departmentObject, 
		[
			"Department Head" : newEmployee,
			Status : "Up and running"
		]
	)

```

## Some more good news

### Flexible parameters
As mentioned before IM was created to be as easy and flexible as possible, for example all methods that expect an Insight object as Input will accept an object key, object id or a an objectBean and take if from there. For example, all of the statements below are equivalent: 

```groovy
im.updateObjectAttributes(departmentObject, Status , "Up and running")
im.updateObjectAttributes("KEY-123", Status , "Up and running")
im.updateObjectAttributes(123, Status , "Up and running")
```
### Documentation and Logs > Comments
IM is not heavily commented, rather it has quite extensive logging and the methods have individual documentation that can be read either by looking directly in IM or if you are using Intellij you can highlight the method and press F1:
<img src="https://raw.githubusercontent.com/wiki/Riada-AB/InsightManager/Images/ImMethodDocumentation.png" width="600">


To enable all IM logs:
```groovy
import org.apache.log4j.Level

InsightManagerForScriptrunner im = new InsightManagerForScriptrunner()  
im.log.setLevel(Level.ALL)
```


### Additional functionality
IM comes with a couple extra tricks up it´s sleeve, here is a short summary of them:

#### IM can be run in a read only mode

Simply by setting. IM in read only mode you can safely run your script and know that imports wont be triggered, objects wont get created or updated
```groovy
im.readOnly = true //false is defualt
```

#### Support for a service user
Scripts in JIRA are often triggered by limited users such as JSD Customers but the scripts might still have to escalate its privileges to CRUD Insight objects, because of this IM allows you to set a service user and automatically escalate to that user.
```groovy
im.setServiceAccount("nisse.hult")
im.autoEscalate = true //Default is true
```

#### Render objects as HTML
Sometimes you might need to display an Insight object in places and ways that Insight and JIRA doesn't support out of the box, this is where IM and ScriptRunner makes a great match. With for example ScriptRunner [behaviors](https://scriptrunner.adaptavist.com/latest/jira/behaviours-overview.html?utm_source=product-help) you could use IM to display some or all of an objects attributes as a field description in a JSD portal.
```groovy
im.renderObjectToHtml(serverObject)
```

<img src="https://raw.githubusercontent.com/wiki/Riada-AB/InsightManager/Images/ImRenderObjectToHtmlDocumentation.png" width="500">



## How do I get started?


Have a look at the [setup guide](https://github.com/Riada-AB/InsightManager/wiki/Setup-and-Upgrade) to get IM setup in your environment and then dig in to the [documentation](https://github.com/Riada-AB/InsightManager/wiki/Method-Overview).

