# *1* Instalção e configuração no Ubuntu (WSL2)

Pacotes de packages.ros.org
```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

Adiconar a chave
```bash
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

Atulizar lista de pacotes
```bash
sudo apt update
```

Intalanção da versão completa
```bash
sudo apt install ros-melodic-desktop-full
```

Verificação do pacotes disponíveis
```bash
apt search ros-melodic
```

Configuração de variáveis (para o zsh)
```bash
echo "source /opt/ros/melodic/setup.zsh" >> ~/.zshrc
source ~/.zshrc
```

Instalação de dependências
```bash
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```
Instalando rosdep para o python
```bash
sudo apt install python-rosdep
```

Inicializando o rosdep
```bash
sudo rosdep init
rosdep update
```

# Gerenciando o ambiente
Verificando as váriais do ROS
```bash
➜  ros git:(master) ✗ printenv | grep ROS
ROS_DISTRO=melodic
ROS_ETC_DIR=/opt/ros/melodic/etc/ros
ROS_PACKAGE_PATH=/opt/ros/melodic/share
ROS_PYTHON_VERSION=2
ROS_VERSION=1
ROS_ROOT=/opt/ros/melodic/share/ros
ROS_MASTER_URI=http://localhost:11311
ROSLISP_PACKAGE_DIRECTORIES=
```

Os arquivos de setup estão
```bash
source /opt/ros/melodic/setup.zsh
```
# Criando um ROS Workspace
Seguindo os passos abaixo:
```bash
➜  ros git:(master) ✗ cd activity1
➜  activity1 git:(master) ✗ mkdir -p ./catkin_ws/src
➜  catkin_ws git:(master) ✗ catkin_make
```

Arquivos de setup

```bash
➜  catkin_ws git:(master) ✗ source devel/setup.zsh

```

Verificando que a variável de ambiente ROS_PACKAGE_PATH inclui o diretório atual
```bash
➜  catkin_ws git:(master) ✗ echo $ROS_PACKAGE_PATH
/home/igor/projects/ros/activity1/catkin_ws/src:/opt/ros/melodic/share
```

# *2* Navegando no sistema de arquivos ROS
Instalando o tutorial
```bash
➜  catkin_ws git:(master) ✗ sudo apt install ros-melodic-ros-tutorials
```
## rospack
O comando rospack permite obter informações sobre pacotes. No caso abaixo usando o comando find para obter a localização de roscpp
```
➜  catkin_ws git:(master) ✗ rospack find roscpp
/opt/ros/melodic/share/roscpp
```

## roscd
O roscd permite a alteração do diretório diretamente para um pacote ou uma pilha específica.

```bash
➜  catkin_ws git:(master) ✗ roscd roscpp
➜  roscpp
➜  roscpp pwd
/opt/ros/melodic/share/roscpp
```

Subdiretórios
```bash
➜  roscpp roscd roscpp/cmake
➜  cmake pwd
/opt/ros/melodic/share/roscpp/cmake
➜  cmake
```

## rosls
O rosls permite a listagem diretamente em um pacote pelo nome em vez de pelo caminho absoluto, usando o `ls` do bash.

```bash
➜  ~ rosls roscpp_tutorials
cmake  launch  package.xml  srv
```

# *3* Criando um pacote ROS
Na vegando para o diretório /src do catkin_ws
```
➜  catkin_ws git:(master) ✗ cd src
```

Usando o catkin_create_pkg para criar um novo pacote chamado 'beginner_tutorials' que depende de std_msgs, roscpp e rospy

```bash
➜  src git:(master) ✗ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
Created file beginner_tutorials/package.xml
Created file beginner_tutorials/CMakeLists.txt
Created folder beginner_tutorials/include/beginner_tutorials
Created folder beginner_tutorials/src
Successfully created files in /home/igor/projects/ros/activity1/catkin_ws/src/beginner_tutorials. Please adjust the values in package.xml.
```

O comando acima criará uma pasta begin_tutorials que contém um package.xml e um CMakeLists.txt, que foram parcialmente preenchidos com as informações que foram fornecidas ao catkin_create_pkg (std_msgs, rospy e roscpp).

## Construindo um workspace catkin e fornecendo o arquivo de configuração

Construindo os pacotes em um workspace catkin
```bash
➜  catkin_ws git:(master) ✗ catkin_make
```
Tem ir para o diretório `catkin_ws`.

Depois que o workspace foi construído, ele criou uma estrutura semelhante na subpasta devel.

Para adicionar o workspace no ambiente ROS, é preciso obter o arquivo de configuração gerado:
```bash
➜  activity1 git:(master) ✗ . catkin_ws/devel/setup.zsh
```

## Dependências do pacote
### Dependências de primeira ordem
```bash
➜  activity1 git:(master) ✗ rospack depends1 beginner_tutorials
roscpp
rospy
std_msgs
```

### Dependências indiretas
Uma dependência também terá suas próprias dependências., como é possível observar abaixo
```bash
➜  beginner_tutorials git:(master) ✗ rospack depends1 rospy
genpy
roscpp
rosgraph
rosgraph_msgs
roslib
std_msgs
```

Com o rospack é possível determinar recursivamente todas as dependências aninhadas.
```bash
➜  beginner_tutorials git:(master) ✗ rospack depends beginner_tutorials
cpp_common
rostime
roscpp_traits
roscpp_serialization
catkin
genmsg
genpy
message_runtime
gencpp
geneus
gennodejs
genlisp
message_generation
rosbuild
rosconsole
std_msgs
rosgraph_msgs
xmlrpcpp
roscpp
rosgraph
ros_environment
rospack
roslib
rospy
```

## Personalizando o pacote

### Personalizando o package.xml

Mudar as tags `description`, `maintainer` e `license` como é possível observar em [activity1/catkin_ws/src/beginner_tutorials/package.xml](activity1/catkin_ws/src/beginner_tutorials/package.xml).

# *4* Construindo um pacote ROS
Executando o comando catkin_make fazer o build.
```bash
➜  catkin_ws git:(master) ✗ catkin_make
```
Com isso a build do pacote ROS foi realizada.


# *5* Entendendo os Nodes ROS
## roscore
Executando o comando abaixo para iniciar
```
➜  catkin_ws git:(master) ✗ roscore
```

## Usando o rosnode
Alguns comandos para executar em novo terminal: `rosnode list`, `info /<dir>`

## Usando o rosrun
Em um novo terminal execute o comando abaixo:
```
rosrun turtlesim turtlesim_node
```
*obs*: Não consegui executar esse passo por conta do WSL2.

# *6* Entendendo ROS Topics

# *7* Entendendo ROS Services e Parameters

# *8* Usando rqt_console e roslaunch

# *9* Usando rosed para editar arquivos no ROS
## rosed
Comando utilizado para editar um arquivo, como observado abaixo:
```
➜  ~ rosed roscpp Logger.msg
```
O edito de default é o `vim`.

## Usando rosed com tab completion
Basta digitar o comando da seguinte forma
```
rosed roscpp <tab><tab>
```

## Editor
Para verificar o editor padrão
```bash
➜  ~ echo $EDITOR
```

Para mudar o editor
```bash
➜  ~ export EDITOR='code -w'
➜  ~ echo $EDITOR
nano -w
```

# *10* Criando um ROS msg e srv


# *12* Publisher e Subscriber (Python)
## Publisher Node
Vá para o diretório `beginner_tutorials`. Em seguida crie o diretório `scripts` e vá para dentro dele.
```bash
➜  beginner_tutorials git:(master) ✗ mkdir scripts && cd scripts
```
Faça o download do arquivo `talker.py` com seguinte comando
```bash
➜  scripts git:(master) ✗ wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/talker.py
```
Mude a forma de acesso do arquivo
```bash
➜  scripts git:(master) ✗ chmod +x talker.py
```

Antes de executar o script, adicione a seguinte linhas no arquivo [CMakeLists.txt](activity1/catkin_ws/src/beginner_tutorials/CMakeLists.txt)
```txt
catkin_install_python(PROGRAMS scripts/talker.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```
Execute o `roscore` e em outro terminal execute o script da seguinte forma
```bash
➜  scripts git:(master) ✗ ./talker.py
```
Se tudo ocorreu bem a saída será como
```bash
[INFO] [1656375955.040562]: hello world 1656375955.04
[INFO] [1656375955.141397]: hello world 1656375955.14
[INFO] [1656375955.240904]: hello world 1656375955.24
[INFO] [1656375955.341278]: hello world 1656375955.34
[INFO] [1656375955.440994]: hello world 1656375955.44
...
```

## Subscriber Node

Faça o download do arquivo `listener.py` com seguinte comando
```bash
➜  scripts git:(master) ✗ pwd
/home/igor/projects/ros/activity1/catkin_ws/src/beginner_tutorials/scripts
➜  scripts git:(master) ✗ wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/listener.py

```

Antes de executar o script, adicione a seguinte linhas no arquivo [CMakeLists.txt](activity1/catkin_ws/src/beginner_tutorials/CMakeLists.txt)
```txt
catkin_install_python(PROGRAMS scripts/talker.py scripts/listener.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```
Mude a forma de acesso do arquivo
```bash
➜  scripts git:(master) ✗ chmod +x listener.py
```

Vá para o diretório `catkin_ws`, e faça a build do nodes como abaixo
```bash
➜  catkin_ws git:(master) ✗ catkin_make
```

Em seguinda, execute o script `talker.py` e o `listener.py`. Se tudo ocorre como esperado, ss logs devem ser como abaixo
```
➜  scripts git:(master) ✗ ./listener.py
[INFO] [1656377127.908178]: /listener_4697_1656377086377I heard hello world 1656377127.91
[INFO] [1656377128.012565]: /listener_4697_1656377086377I heard hello world 1656377128.01
[INFO] [1656377128.109417]: /listener_4697_1656377086377I heard hello world 1656377128.11
[INFO] [1656377128.209804]: /listener_4697_1656377086377I heard hello world 1656377128.21
[INFO] [1656377128.315583]: /listener_4697_1656377086377I heard hello world 1656377128.31
[INFO] [1656377128.415404]: /listener_4697_1656377086377I heard hello world 1656377128.41
[INFO] [1656377128.511540]: /listener_4697_1656377086377I heard hello world 1656377128.51
...
```

*obs*: Se tentar executar `listener.py` sem executar o `talker.py` nada vai acontecer.
