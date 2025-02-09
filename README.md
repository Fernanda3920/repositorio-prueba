# Modos de Direccionamiento

Un **modo de direccionamiento** define cómo calcular la dirección efectiva de un operando en memoria, utilizando la información contenida en registros y/o constantes dentro de una instrucción de la máquina o en otra parte. Un modo de direccionamiento que en una arquitectura se trata de cierta manera, en otra puede requerir la combinación de dos o más modos para lograr la misma funcionalidad.

## Modos de Direccionamiento Existentes

La mayoría de las arquitecturas **RISC** emplean solo cinco modos de direccionamiento simples, mientras que algunas **CISC**, como el **DEC VAX**, cuentan con más de una docena, algunos de ellos muy complejos. Cuando hay pocos modos, estos suelen estar codificados directamente dentro de la instrucción (ej. **IBM/390**). Sin embargo, cuando hay muchos, es común reservar un campo específico en la instrucción para indicar el modo de direccionamiento.

### Tipos de Direccionamiento
Ejemplos en Ensamblador x86.

#### **Implícito**
En este modo, la dirección de los operandos no se especifica explícitamente, ya que está determinada por la propia instrucción. Se considera que este no es propiamente un modo de direccionamiento, dado que no se indica un operando de manera explícita.
```assembly
IN    ; La instrucción IN recibe datos de un puerto de entrada (operando implícito)
CLC   ; La instrucción CLC limpia el flag de acarreo (operando implícito)
```
#### **Inmediato**
El operando está incluido directamente en la instrucción, en lugar de una dirección de memoria. Es un valor fijo, por lo que se usa comúnmente para definir constantes y variables.
```assembly
MOV AX, 10   ; Mueve el valor inmediato 10 al registro AX
ADD BX, 5    ; Suma el valor inmediato 5 al registro BX
```
#### **Directo o Absoluto**
El campo del operando en la instrucción contiene la dirección en memoria donde se encuentra el dato. Si hace referencia a un registro, el operando se encuentra en dicho registro (**direccionamiento directo a registro**). Si hace referencia a una posición de memoria, el operando está almacenado en esa dirección específica (**direccionamiento directo a memoria**).
```assembly
MOV AX, [1000H]  ; Mueve el contenido de la dirección de memoria 1000H al registro AX
```
#### **Indirecto**
El campo del operando contiene una dirección de memoria, que a su vez almacena la dirección efectiva del operando. Puede ser:
- **Indirecto a registro:** La dirección efectiva está en un registro.
- **Indirecto a memoria:** La dirección efectiva está en una posición de memoria.
```assembly
MOV SI, 2000H   ; Se guarda la dirección 2000H en el registro SI
MOV AX, [SI]    ; Se carga en AX el valor almacenado en la dirección 2000H
```
Existe también el **modo indirecto recursivo**, en el cual la dirección almacenada contiene otro campo de registro para indexación, y este proceso puede repetirse hasta encontrar una dirección sin un bit indirecto.

#### **De Registro**
Los operandos residen dentro de la CPU y se especifican directamente mediante registros.
```assembly
MOV AX, BX   ; Copia el contenido del registro BX en AX
ADD CX, DX   ; Suma el contenido de DX a CX
```

#### **Indirecto mediante Registros**
El campo del operando contiene un identificador de registro, donde se encuentra la dirección efectiva del operando.
```assembly
MOV BX, [SI]  ; Se carga en BX el contenido de la dirección almacenada en SI
```
#### **De Desplazamiento**
Combina el modo directo e indirecto a través de registros, requiriendo dos campos: uno referencia un registro y el otro define un desplazamiento. Existen tres variantes:

- **Relativo:** Se suma el valor del campo de dirección a la dirección de la instrucción en ejecución.
- **Por registros base:** Se direcciona un registro y se le suma un desplazamiento.
- **Indexado:** Se usa una dirección de memoria y un registro con un desplazamiento. Puede ser:
  - **Preindexado:** Se suma el valor del registro y el campo de dirección antes de acceder.
  - **Postindexado:** Primero se obtiene el valor de la dirección del campo y luego se le suma el valor del registro.
```assembly
JMP SHORT +10  ; Salta a la dirección actual + 10 bytes
```
#### **De Pila**
Se usa cuando el operando está en la memoria y en la cima de la pila, utilizando el **SP (Stack Pointer)** como puntero de direccionamiento.
```assembly
PUSH AX    ; Guarda el valor de AX en la pila
POP BX     ; Recupera el último valor almacenado en la pila en BX
```
#### **Relativo a un Registro Base**
La dirección efectiva se calcula sumando el contenido de un registro base con un desplazamiento positivo.
```assembly
MOV AX, [BP+6]  ; Obtiene el valor en BP + 6 (típico en parámetros de función)
```
#### **Relativo a un Registro Índice**
El contenido del registro índice representa un desplazamiento a partir de una dirección de memoria proporcionada como argumento en la instrucción.
```assembly
MOV AX, [1000H + SI]  ; Obtiene el valor en dirección 1000H + SI
```
#### **Relativo al Contador de Programa**
Usa el **PC (Program Counter)** como registro base para calcular la dirección de memoria.
```assembly
LDR R0, [PC, #4]  ; Carga un valor en R0 desde una dirección relativa a PC
```
