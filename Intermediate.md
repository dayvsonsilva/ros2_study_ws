# Anotações: Intermediate


[Creating an action](http://docs.ros.org/en/galactic/Tutorials/Actions/Creating-an-Action.html)  

Você aprendeu sobre ações anteriormente no tutorial Entendendo as ações do ROS 2. Assim como os outros tipos de comunicação e suas respectivas interfaces (tópicos/msg e serviços/srv), você também pode definir ações personalizadas em seus pacotes. Este tutorial mostra como definir e construir uma ação que você pode usar com o servidor de ação e o cliente de ação que você escreverá no próximo tutorial.


o principal arquivo deste pacote é o Fibonacci.action.
```
int32 order # Request
---
int32[] sequence # Result
---
int32[] partial_sequence # Feedback
```  

Arquivos editados

CMakeLists.txt
package.xml


[Writing an action server and client (C++)]()  

[Writing an action server and client (Python)]()  

[Composing multiple nodes in a single process]()  

[Using colcon to build packages]()  

[Monitoring for parameter changes (C++)]()  

[Launch Tutorials]()  

[tf2 Tutorials]()  

[URDF Tutorials]()  
