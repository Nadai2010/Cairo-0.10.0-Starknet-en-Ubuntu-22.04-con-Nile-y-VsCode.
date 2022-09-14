# Nadai-Cairo

Configurando el entorno Cairo 0.10.0 & Starknet para Ubuntu 22.04 usando Nile y VsCode.

Instalación:
Recomendamos trabajar dentro de un entorno virtual Python, pero también puede instalar el paquete Cairo  
directamente. Para crear e ingresar al entorno virtual, abrimos una terminal (Ctrl+Alt+T):
```bash
python3.9 -m venv ~/cairo_venv
source ~/cairo_venv/bin/activate
```
Asegúrese de que venv esté activado; debería verlo (cairo_venv)en el indicador de la línea de comandos.

*En caso de error, pruebe con los comandos siguientes para añadir la ultima version python3.9.
Pero aun me daba error al hacer el Nile Compile.
```bash
sudo apt install -y python3.9 python3.9-dev python3.9-venv python3-pip
python3.9 -m venv ~/cairo_venv
source ~/cairo_venv/bin/activate
```
*Se arregló todo instalando de nuevo la libreria de openzeppelin, que más abajo usaremos para instalar Nile
en VsCode.
```bash
pip install cairo-lang cairo-nile openzeppelin-cairo-contracts==0.4.0b
```

Asegúrese de que venv esté activado; debería verlo (cairo_venv)en el indicador de la línea de comandos.


----------------------------------------------------------------------------------------------------------------------------------------------


*En caso de error a instalar python3.9

-La instalación de Python 3.9 en Ubuntu con apt es un proceso sencillo, que además se puede llevar a cabo 
de forma muy rápida. Para empezar abrimos una terminal (Ctrl+Alt+T) y vamos a actualizar la lista 
de paquete disponibles desde los repositorios:
```bash
sudo apt update
```
-A continuación vamos a instalar los requisitos previos necesarios, si es que todavía no los tenemos 
instalados:
```bash
sudo apt install software-properties-common
```
-Lo siguiente que haremos será añadir el PPA de deadsnakes a la lista de fuentes de nuestro sistema:
```bash
sudo add-apt-repository ppa:deadsnakes/ppa
```
-Instalar python 3.9
```bash
sudo apt install python3.9
```
-Finalizada la instalación, verificaremos que la instalación fue correcta escribiendo en la terminal:
```bash
python3.9 --version
```

---------------------------------------------------------------------------------------------------------------------------------------------------------


-Asegúrese de poder instalar los siguientes paquetes pip: ecdsa, fastecdsa, sympy (usando ). 
 ```bash
pip3 install ecdsa fastecdsa sympy
```
*Si le pide actualizar pip version puede proceder sustituyendo su via, versión o cambiando su usuario en la 
siguiente:
```bash
/home/(usuario)/cairo_venv/bin/python3.9 -m pip install --upgrade pip
```
-Luego:
```bash
sudo apt install -y libgmp3-dev
```
Si todo ha salido bien con estas instrucciones, perfecto. Es muy probable que no salga a la primera, 
los (*) añadidos has sido problemas que han ido surgiendo y como se han solucionado.

---------------------------------------------------------------------------------------------------------------------------------------------------

VsCode para tu Cairo usando Nile (Nile seria como un Hardhat en Solidity)

Primero recomendamos instalar las dos extensiones de Cairo & Cairo language support for Starknet.
  1. Fue creada por starkware
  2. Fue creada por Erik Lau quien es parte de OpenZepellin

-Crea una carpeta para tu proyecto y accede con tu terminal de Vscode:
```bash
mkdir myproject
cd myproject
```
-Volvemos a crear un entorno ENV. y activarlo (Dudas repasar paso Instalación):
```bash
python3 -m venv env
source env/bin/activate
```
-Instalación de Nile:
```bash
pip install cairo-nile
pip install cairo-lang cairo-nile openzeppelin-cairo-contracts==0.4.0b
```
-Aqui podemos revisar la versión de Cairo:
```bash
cairo-compile --version
```
(cairo-compile 0.10.0) // Más reciente

-Ejecute init para iniciar un nuevo proyecto. Nile creará la estructura de directorios del proyecto e 
instalará dependencias como el idioma Cairo, una red local y un marco de prueba.
```bash
nile init
```
🗄 Instalando Cairo                                                                                                                      
✨ ¡Cairo instalado con éxito!                                                                                                                           
✅ Dependencias instaladas con éxito                                                                                       
🗄 Creación del árbol de directorios del proyecto                                                                                    
⛵️ ¡Proyecto Nile listo! Intente ejecutar: 


-Ahora ejecutemos un nodo de prueba local de StarkNet para que podamos trabajar localmente y déjelo en 
ejecución para el resto de los pasos, abriendo una terminal diferente para continuar. 
*Tuve que ejecutar de nuevo ENV.
```bash
source env/bin/activate
nile node
```
-Saldra un mensaje en vuestra terminal con varia cuentas y private key

Initial balance of each account: 1000000000000000000000 WEI
Seed to replicate this account sequence: None
WARNING: Use these accounts and their keys ONLY for local testing. DO NOT use them on mainnet or other live 
networks because you will LOSE FUNDS.

 * Listening on http://127.0.0.1:5050/ (Press CTRL+C to quit)

-Ahora realizamos un test:
```bash
nile test
```
🤖 Running all test Cairo contracts in the contracts directory

Usa un preajuste de Smart de una de las guias exportadas.

-Cambie el nombre de contracts/contract.cairo a contracts/test.cairo y reemplace su contenido con:

func main() {                                                                                                           
    [ap] = 1000, ap++;                                                                                                 
    [ap] = 2000, ap++;                                                                                                         
    [ap] = [ap - 2] + [ap - 1], ap++;                                                                                                  
    ret;                                                                                                                                                
}                                                                                                                               

-¡Eso es todo! Ese es nuestro contrato. Y por fin probaremos la compilacion si está correcto, en mi caso ha 
sido posible después de muchas pruebas, fallos y * en este documento.
```bash
nile compile
```
🤖 Compiling all Cairo contracts in the contracts directory                                                                                      
🔨 Compiling contracts/test.cairo                                                                                                                      
✅ Done


*Aqui nos daba error, el cual se ha solucionado instalando la libreria de OpenZepellin añadida más arriba.

*Durante la instalación tuve diversos errores, algunos se solucionaron probando:
```bash
sudo apt update
sudo apt upgrade
```
*Tambien instale la librería (Bump version to v0.4.0b)
```bash
sudo pip install openzeppelin-cairo-contracts
```

Compilar: (asegúrese de que todos los comandos se ejecuten en el entorno virtual)
*-En este paso he tenido varios problemas de ruta, asegurarse que están en la carpeta que se ha creado
el test.cairo.
```bash
cairo-compile test.cairo --output test_compiled.json
```
-Luego procederemos a correr el test.
```bash
cairo-run \
  --program=test_compiled.json --print_output \
  --print_info --relocate_prints
```
-Una muestra de como se veria.

Number of steps: 4 (originally, 4)                                                                                                            
Used memory cells: 11                                                                                                               
Register values after execution:                                                                                                                        
pc = 12                                                                                                                                            
ap = 12                                                                                                                                                   
fp = 12

-Puede abrir el rastreador de El Cairo proporcionando el indicador --tracer a cairo-run.
 Luego ábralo en http://localhost:8100/.
```bash
 cairo-run \
  --program=test_compiled.json --print_output \
  --print_info --relocate_prints --tracer
```
-Una muestra de como se veria.

Number of steps: 4 (originally, 4)                                                                                                                  
Used memory cells: 11                                                                                                                                    
Register values after execution:                                                                                                                  
pc = 12                                                                                                                                              
ap = 12                                                                                                                                             
fp = 12                                                                                                                                                


Running tracer on http://localhost:8100/

Aqui damos por concluido las pruebas de Nile y local ahora pasaremos a configurar red he intetaremos 
aprender tutoriales basicos de Hello Cairo y Starknet.

------------------------------------------------------------------------------------------------------------------

Configurando la red:

-En este tutorial, utilizaremos la CLI de StarkNet (interfaz de línea de comandos) para interactuar con 
StarkNet. Para indicarle a la CLI que funcione con la red de prueba StarkNet, puede agregar la 
--network=alpha-goerli, a cada comando o simplemente configurar la STARKNET_NETWORK variable de 
entorno de la siguiente manera:
```bash
export STARKNET_NETWORK=alpha-goerli
```
-Elegir un proveedor de billetera:

A diferencia de Ethereum, que distingue entre cuentas de propiedad externa (EOA) y contratos, StarkNet no 
tiene esta distinción.

En cambio, una cuenta está representada por un contrato implementado que define la lógica de la cuenta, en 
particular, el esquema de firma que controla quién puede emitir transacciones desde ella.

Para interactuar con StarkNet, deberá implementar un contrato de cuenta. En este tutorial, utilizaremos una 
versión ligeramente modificada del estándar de OpenZeppelin para el contrato EOA (por el momento, la firma 
se calcula de manera diferente). Establezca la STARKNET_WALLETvariable de entorno de la siguiente manera:
```bash
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount
```
Crear una cuenta:
-Ejecute el siguiente comando para crear una cuenta:
```bash
starknet deploy_account
```
-La salida debe parecerse a:

Sent deploy account contract transaction.

NOTE: This is a modified version of the OpenZeppelin account contract. The signature is computed
differently.

Contract address: 0x0180d3921df180beb87946532e053cc1beda132ffdf2b0e35c33880374ab8930                                 
Public key: 0x03206b0ea93173079bb51bf185a63b0c4598ea5ca64aa7063d66b080c1dadb91                                                                     
Transaction hash: 0x4e29dbe4f234340c0a710d77a3b6bb705c85d716bbd0f1f80d4742d5aa24290


También puede especificar un nombre para su cuenta --account=my_account si desea mantener varias cuentas. 
Si no se especifica, se utiliza la cuenta predeterminada (denominada __default__).

La STARKNET_WALLET variable de entorno le indica a StarkNet CLI que use su cuenta en los comandos y
si desea realizar una llamada directa a un contrato, sin pasar por su contrato de cuenta, puede pasar el 
argumento a la CLI, que anula la variable.starknet invokestarknet call--no_walletSTARKNET_WALLET

Advertencia : el uso de los proveedores de billetera integrados que forman parte del cairo-langpaquete 
( starkware.starknet.wallets...) no es seguro (por ejemplo, la clave privada puede mantenerse sin cifrar y 
sin respaldo en su directorio de inicio). Solo debe usarlos si no le preocupa demasiado perder el acceso a 
sus cuentas (por ejemplo, con fines de prueba). Además, no se implementan con el patrón de proxy, por lo que
no se pueden actualizar y pueden dejar de funcionar en futuras versiones de StarkNet.

Transferir Goerli ETH a la cuenta:

Para ejecutar transacciones en StarkNet, deberá tener ETH en su cuenta L2 (para pagar las tarifas de 
transacción). Puede adquirir L2 ETH de las siguientes maneras:

Use StarkNet Faucet (https://faucet.goerli.starknet.io/#) para obtener pequeñas cantidades de ETH
directamente en la cuenta que acaba de crear. Esto debería ser suficiente para transacciones simples.
Aqui adjunto el Hash de la cuenta que se ha creado recibiendo su faucet ETH. (Más de 25 minutos.)
Tambien por curiosidad el hash de la acpetacion el L1 Test Goerli

https://goerli.voyager.online/tx/0x81d0fc173c9addf61bb3127ed06164a1a1fc2593dd198228543df4a75f709a
https://goerli.etherscan.io/tx/0x4456fa63dc557ca8a5bcae31553d313c1d4ba3eab91513e8cee38f62c514ace0


Utilice StarkGate, el puente StarkNet L2 ( contrato L1 / aplicación web ) para transferir su Goerli L1 ETH 
existente hacia y desde la cuenta L2.
