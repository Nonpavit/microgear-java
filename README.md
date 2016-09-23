#Microgear-java-----------microgear-java is a client library for Java The library is used to connect application code or hardware with the NETPIE Platform's service for developing IoT applications. For more details on the NETPIE Platform, please visit https://netpie.io##Installation-----------Refer to the latest version directly from the Maven like this```java<dependency>	<groupId>io.netpie</groupId>	<artifactId>microgear</artifactId>	<version>1.0.0</version></dependency>```<br/>##Usage Example-----------```javaimport io.netpie.microgear.Microgear;import io.netpie.microgear.MicrogearEventListener;public class Example {	final static String appID = "appID";	final static String Key = "Key";	final static String Secret = "Secret";	static Microgear microgear;	static callback callback;	public static void main(String[] args) throws InterruptedException {		microgear = new Microgear();		callback = new callback();		microgear.setCallback(callback);		microgear.connect(appID, Key, Secret);		microgear.Subscribe("/Topic");		int count = 1;		for(;;){			microgear.Publish("/Topic", String.valueOf(count)+".  Test message");			count++;			Thread.sleep(2000);		}	}	static class callback implements MicrogearEventListener{		@Override		public void onConnet() {			// TODO Auto-generated method stub			System.out.println("Microgear is Connected.");			//this.microgear.Publish("/chat", "Hello world#pppppp#qqqqq");		}		@Override		public void onMessage(String topic, String message) {			// TODO Auto-generated method stub			System.out.println(topic+" "+message);		}		@Override		public void onPresent(String token) {			// TODO Auto-generated method stub			System.out.println("Present "+token);		}		@Override		public void onAbsent(String token) {			// TODO Auto-generated method stub			System.out.println("Absent "+token);		}		@Override		public void onDisconnect() {			// TODO Auto-generated method stub			System.out.println("Microgear is Disconnect.");		}		@Override		public void onError(String error) {			// TODO Auto-generated method stub			System.out.println("Error "+error);		}	}}```<br/>##Library Usage-----------###Microgear-----------####Create&Connect**microgear.connect(appID, Key, Secret);**arguments* *appID* `string` - a group of application that microgear will connect to.* *Key* `string` - is used as a microgear identity.* *Secret* `string` - comes in a pair with gearkey. The secret is used for authentication and integrity.<br/>####setCallback**microgear.setCallback(Callback);**arguments* *Callback* `MicrogearEventListener` - event class.<br/>####Setalias**Setalias(Newalias)** microgear can set its own alias, which to be used for others make a function call chat(). The alias will appear on the key management portal of netpie.ioarguments* *Newalias* `string` – name of this microgear.<br/>####Chat**Chat(Name, Message);**arguments* *Name* `string` - name of microgear to which to send a message.* *Message* `string` – message to be sent.<br/>####Publish**Publish(Topic, Message, Retained);**In the case that the microgear want to send a message to an unspecified receiver, the developer can use the function publish to the desired topic, which all the microgears that subscribe such topic will receive a message.arguments* *Topic* `string` - name of topic to be send a message to.* *Message* `string` – message to be sent.* *Retained* `Boolean` - retain a message or not (the default is false)<br/>####Subscribe**Subscribe(Topic);** microgear may be interested in some topic. The developer can use the function subscribe() to subscribe a message belong to such topic. If the topic used to retain a message, the microgear will receive a message everytime it subscribes that topic.arguments* *Topic* `string` - name of the topic to send a message to.<br/>####Unsubscribe*Unsubscribe(Topic);* cancel subscriptionarguments* *Topic* `string` - name of the topic to send a message to.<br/>####Disconnect**microgear.Disconnect();** cancel connect<br/>###Even-----------An application that runs on a microgear is an event-driven type, which responses to various events with the callback function in a form of event function call####Even connectThis event is created when the microgear library successfully connects to the NETPIE platform.```java@Overridepublic void onConnet() {	// TODO Auto-generated method stub	System.out.println("Microgear is Connected.");	//this.microgear.Publish("/chat", "Hello world#pppppp#qqqqq");}```<br/>####Even messageWhen there is an incomming message, this event is created with the related information to be sent via the callback function.```java@Overridepublic void onMessage(String Topic, String Message) {	// TODO Auto-generated method stub	System.out.println(Topic+" "+Message);}```arguments* *Topic* `string` - topic of message* *Message* `string` - message received<br/>####Even presentThis event is created when there is a microgear under the same appid appears online to connect to NETPIE.```java@Overridepublic void onPresent(String Name) {	// TODO Auto-generated method stub	System.out.println("Present "+Name);}```arguments* *Name* `string` - Name of microgear under the same appid appears online<br/>####Even absentThis event is created when the microgear under the same appid appears offline.```java@Overridepublic void onAbsent(String Name) {	// TODO Auto-generated method stub	System.out.println("Absent "+Name);}```arguments* *Name* `string` - Name of microgear under the same appid appears offline<br/>####Even disconnectThis event is created when the microgear library disconnects the NETPIE platform.```java@Overridepublic void onDisconnect() {	// TODO Auto-generated method stub	System.out.println("Microgear is Disconnect.");}```<br/>####Even errorThis event is created when an error occurs within a microgear.```java@Overridepublic void onError(String error) {	// TODO Auto-generated method stub	System.out.println("Error "+error);}```arguments* *error* `string` - Log error message