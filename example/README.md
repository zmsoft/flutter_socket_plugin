# flutter_socket_plugin_example

Demonstrates how to use the flutter_socket_plugin plugin.

## Getting Started

```
import 'package:flutter/material.dart';
import 'package:flutter_socket_plugin/flutter_socket_plugin.dart';

void main() => runApp(App());

class App extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "home",
      home: HomePage(),
      theme: ThemeData(primaryColor: Colors.lightGreen),
    );
  }
}


class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {

  TextEditingController textEditingController = TextEditingController();

  FlutterSocket flutterSocket;
  bool connected = false;
  String _host = "192.168.8.82";
  int _port = 10007;
  String receiveMessage = "";

  @override
  void initState() {
    initSocket();
    textEditingController.addListener((){
      print("input text:${textEditingController.text}");
    });
    super.initState();
  }

  ///
  /// @Method: initSocket
  /// @Parameter:
  /// @ReturnType:
  /// @Description: init socket
  /// @author: waitwalker
  /// @Date: 2019-08-23
  ///
  initSocket() {

    /// init socket
    flutterSocket = FlutterSocket();

    /// listen connect callback
    flutterSocket.connectListener((data){
      print("connect listener data:$data");
    });

    /// listen error callback
    flutterSocket.errorListener((data){
      print("error listener data:$data");
    });

    /// listen receive callback
    flutterSocket.receiveListener((data){
      print("receive listener data:$data");
      if (data != null) {
        receiveMessage = receiveMessage + "\n" + data;
      }
      setState(() {

      });
    });

    /// listen disconnect callback
    flutterSocket.disconnectListener((data){
      print("disconnect listener data:$data");
    });

  }


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("FlutterSocket",style: TextStyle(fontSize: 18,color: Colors.white),),
        backgroundColor: Colors.lightBlue,
      ),
      body: Column(
        children: <Widget>[

          Padding(
            padding: EdgeInsets.only(top: 30,left: 20,right: 20),
            child: Row(
              children: <Widget>[
                Text("Host:",style: TextStyle(fontSize: 22,color: Colors.lightBlueAccent,fontWeight: FontWeight.w600),),
                Padding(padding: EdgeInsets.only(left: 10),),
                Container(
                  decoration: BoxDecoration(
                    border: Border(bottom: BorderSide(color: Colors.redAccent,width: 1.0)),
                  ),
                  child: Text(_host,style: TextStyle(fontSize: 18,color: Colors.black87,fontWeight: FontWeight.w300),),
                ),
                Padding(padding: EdgeInsets.only(left: 30),),
                Text("Port:",style: TextStyle(fontSize: 22,color: Colors.lightBlueAccent,fontWeight: FontWeight.w600),),
                Padding(padding: EdgeInsets.only(left: 10),),
                Container(
                  decoration: BoxDecoration(
                    border: Border(bottom: BorderSide(color: Colors.redAccent,width: 1.0)),
                  ),
                  child: Text("$_port",style: TextStyle(fontSize: 18,color: Colors.black87,fontWeight: FontWeight.w300),),
                ),
              ],
            ),
          ),

          Padding(
            padding: EdgeInsets.only(left: 20,right: 20,top: 30),
            child: Row(
              children: <Widget>[
                RaisedButton(
                  color: Colors.lightBlueAccent,
                  child: Text("Create",style: TextStyle(fontSize: 18,color: Colors.white,fontWeight: FontWeight.w500),),
                  onPressed: () async {
                    await flutterSocket.createSocket(_host, _port, timeout: 20);
                  },
                ),
                Padding(padding: EdgeInsets.only(left: 20)),
                RaisedButton(
                  color: Colors.lightBlueAccent,
                  child: Text("Connect",style: TextStyle(fontSize: 18,color: Colors.white,fontWeight: FontWeight.w500),),
                  onPressed: (){
                    flutterSocket.tryConnect();
                  },
                ),
                Padding(padding: EdgeInsets.only(left: 20)),
                RaisedButton(
                  color: Colors.lightBlueAccent,
                  child: Text("Disconnect",style: TextStyle(fontSize: 18,color: Colors.white,fontWeight: FontWeight.w500),),
                  onPressed: (){
                    flutterSocket.tryDisconnect();
                  },
                ),
              ],
            ),
          ),

          Padding(
            padding: EdgeInsets.only(left: 20,right: 20,top: 30,),
            child: Container(
              height: 150,
              decoration: BoxDecoration(
                border: Border.all(color: Colors.lightBlueAccent,width: 1.0)
              ),
              child: TextField(
                controller: textEditingController,
                decoration: InputDecoration(
                  labelText: "Input content",
                  border: InputBorder.none
                ),
              ),
            ),
          ),

          Padding(
            padding: EdgeInsets.only(left: 20,right: 20,top: 10),
            child: Container(
              alignment: Alignment.centerLeft,
              child: RaisedButton(
                color: Colors.lightBlueAccent,
                child: Text("Send",style: TextStyle(fontSize: 18,color: Colors.white,fontWeight: FontWeight.w500),),
                onPressed: (){
                  flutterSocket.send(textEditingController.text);
                },),
            ),
          ),

          Padding(
            padding: EdgeInsets.only(left: 20,right: 20,top: 30),
            child: Container(
              alignment: Alignment.centerLeft,
              child: RaisedButton(
                color: Colors.lightBlueAccent,
                child: Text("Receive Data",style: TextStyle(fontSize: 18,color: Colors.white,fontWeight: FontWeight.w500),),
                onPressed: (){},),
            ),
          ),

          Padding(
            padding: EdgeInsets.only(left: 20,right: 20,top: 10),
            child: Container(
              height: 150,
              width: MediaQuery.of(context).size.width - 40,
              decoration: BoxDecoration(
                  border: Border.all(color: Colors.lightBlueAccent,width: 1.0)
              ),
              child: Text(receiveMessage,style: TextStyle(fontSize: 15,color: Colors.lightBlueAccent,fontWeight: FontWeight.w500),),
            ),
          ),
        ],
      ),
    );
  }

}
```
