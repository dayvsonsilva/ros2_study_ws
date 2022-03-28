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
```
source /opt/ros/galactic/setup.bash
cd ~/ros2_study_ws
. install/local_setup.bash
```

Agora ao rodar o pacote turtlesim, será utilizado o pacote do ambiente ros2_study_ws(overlay)
```
ros2 run turtlesim turtlesim_node
```

PESQUISAR DIFERENÇA ENTRE:
```
cd ~/ros2_study_ws
. install/local_setup.bash
```
E
```
cd ~/ros2_study_ws
source install/setup.bash
```







