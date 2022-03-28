# Anotações - Beginner: Client Libraries

- [Creating a workspace](http://docs.ros.org/en/galactic/Tutorials/Workspace/Creating-A-Workspace.html)
  

"Um espaço de trabalho é um diretório que contém pacotes do ROS 2. Antes de usar o ROS 2, é necessário fornecer seu espaço de trabalho de instalação do ROS 2 no terminal em que você planeja trabalhar. Isso torna os pacotes do ROS 2 disponíveis para você usar nesse terminal.

Você também tem a opção de fornecer um “overlay” – um espaço de trabalho secundário onde você pode adicionar novos pacotes sem interferir no espaço de trabalho ROS 2 existente que você está estendendo, ou “underlay”. Seu underlay deve conter as dependências de todos os pacotes em seu overlay. Os pacotes em sua sobreposição substituirão os pacotes na subjacência. Também é possível ter várias camadas de subjacências e sobreposições, com cada sobreposição sucessiva usando os pacotes de suas subjacências pai. "

Para cada novo terminal aberto deve ser definido o ambiente de execução
``` shell
source /opt/ros/galactic/setup.bash
```

``` shell
mkdir -p ~/dev_ws/src
cd ~/dev_ws/src
```

Para o repositorio atual o ambiente de trabalho é chamado de ros2_study_ws, os pacotes utilizados no ambiente de trabalho devem estar no diretorio /src.  

Como referencia será utilizado o repositorio ros_tutorials.

``` shell
cd ros2_study_ws/src
git clone https://github.com/ros/ros_tutorials.git -b galactic-devel
```

Instalando depedencias
``` shell
# cd if you're still in the ``src`` directory with the ``ros_tutorials`` clone
cd ..
rosdep install -i --from-path src --rosdistro galactic -y
```

"Os pacotes declaram suas dependências no arquivo package.xml (você aprenderá mais sobre pacotes no próximo tutorial). Este comando percorre essas declarações e instala as que estão faltando. Você pode aprender mais sobre o rosdep em outro tutorial (em breve)."


Build workspace
`` shell
source /opt/ros/galactic/setup.bash
cd ros2_study_ws
colcon build
```

Outros argumentos úteis para a compilação do colcon:

--packages-up-to compila o pacote que você deseja, além de todas as suas dependências, mas não todo o espaço de trabalho (economiza tempo)

--symlink-install evita que você tenha que reconstruir toda vez que ajustar scripts python

--event-handlers console_direct+ mostra a saída do console durante a compilação (pode ser encontrado no diretório de log) 



Source o ambiente de rabalho(overlay)
``` shell
source /opt/ros/galactic/setup.bash
cd ~/ros2_study_ws
. install/local_setup.bash
```

Agora ao rodar o pacote turtlesim, será utilizado o pacote do ambiente ros2_study_ws(overlay)
``` shell
ros2 run turtlesim turtlesim_node
```

PESQUISAR DIFERENÇA ENTRE:
``` shell
cd ~/ros2_study_ws
. install/local_setup.bash
```
E
``` shell
cd ~/ros2_study_ws
source install/setup.bash
```

- [Creating your first ROS 2 package](http://docs.ros.org/en/galactic/Tutorials/Creating-Your-First-ROS2-Package.html)

"1 O que é um pacote ROS 2?
Um pacote pode ser considerado um contêiner para seu código ROS 2. Se você quiser instalar seu código ou compartilhá-lo com outras pessoas, precisará organizá-lo em um pacote. Com os pacotes, você pode liberar seu trabalho do ROS 2 e permitir que outros o construam e usem facilmente.

A criação de pacotes no ROS 2 usa ament como seu sistema de compilação e colcon como sua ferramenta de compilação. Você pode criar um pacote usando CMake ou Python, que são oficialmente suportados, embora existam outros tipos de compilação. "

Tutorial abordando uso do CMake

Estrutura de um pacote ROS2
my_package/
     CMakeLists.txt
     package.xml

Onde:

package.xml - arquivo contendo meta-informações sobre o pacote

CMakeLists.txt - arquivo que descreve como construir o código dentro do pacote 

NÂO UTILIZAR PACOTES ANINHADOS!!!!

my_package_1/
     CMakeLists.txt
     package.xml
     my_package_2/
        CMakeLists.txt
        package.xml


Padrão de organização de pacotes

workspace_folder/
    src/
      package_1/
          CMakeLists.txt
          package.xml

      package_2/
          setup.py
          package.xml
          resource/package_2
      ...
      package_n/
          CMakeLists.txt
          package.xml

Criando um pacote CMake
``` shell
ros2 pkg creat -- build-type ament_cmake <package_name>
```

Utilizando o parametro  --node-name para gerar um executavel de teste
``` shell
ros2 pkg create --build-type ament_cmake --node-name my_node my_package
```

Após criar pacote deve ser realizado a compilação
```
colcon bulid
```

Executando o arquivo hello_world gerado com o parametro --node-name
``` shell
ros2 run my_package my_node
```
Retorno:
``` shell
CMakeLists.txt  include  package.xml  src
```