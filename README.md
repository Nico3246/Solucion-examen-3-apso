## Documento 3 – Soluciones para “wuolah‐free‐PruebashellL31920.pdf” (Grupo L3)

_Este documento agrupa las soluciones para la prueba del Grupo L3, donde se utiliza tanto rutas relativas como absolutas (según se indique) y se solicita la creación de guiones adicionales. Se asume la estructura en ~/pruebaL3._

1. **Cambiar el passwd de la cuenta:**  
   ```bash
   passwd
   ```

2. **Crear la estructura de directorios usando un único comando (con rutas relativas):**  
   (Se requiere: pruebaL3 y, dentro, basura, guion, varios y temp.)
   ```bash
   mkdir -p pruebaL3/{basura,guion,varios,temp}
   ```

3. **Modificar permisos de pruebaL3 y de los directorios varios y guion:**  
   - Para pruebaL3: El propietario tiene todos los permisos; grupo y otros solo ejecución.
     ```bash
     chmod 711 pruebaL3
     ```
   - Para varios y guion (quitar lectura y escritura a grupo y otros):
     ```bash
     chmod 111 pruebaL3/varios pruebaL3/guion
     ```

4. **Copiar (sin usar find) los ficheros de /home/so/ficheros cuyo nombre contenga un número en la tercera posición, al directorio varios (usando rutas absolutas):**  
   ```bash
   cp /home/so/ficheros/??[0-9]* /ruta/completa/a/pruebaL3/varios
   ```
   (Reemplace /ruta/completa/a por la ruta absoluta real.)

5. **Cambiar al directorio pruebaL3 y mover, sin usar find, los ficheros de varios que terminen en “bak” a basura (usando rutas relativas):**  
   ```bash
   cd pruebaL3
   mv varios/*bak basura
   ```

6. **Borrar el directorio basura:**  
   ```bash
   rmdir basura
   ```  
   (O `rm -r basura` si no está vacío.)

7. **Hacer que la ruta al directorio guion y al directorio actual estén en el PATH al iniciar sesión:**  
   Por ejemplo, en el fichero de configuración:
   ```bash
   export PATH=$PATH:$(pwd)/guion:$(pwd)
   ```

8. **Configurar que al iniciar sesión se cree la variable GUION (con la ruta absoluta a guion) y el alias cdvarios (que cambie a varios):**  
   En el .bashrc, agregar:
   ```bash
   export GUION=$(pwd)/guion
   alias cdvarios='cd $(pwd)/varios'
   ```

9. **Crear un fichero en varios llamado nodir que contenga los nombres (sólo el nombre) de todos los ficheros no directorios a partir de /home que:**
   - Empiecen por una letra (mayúscula o minúscula).
   - Tengan “e” como segundo carácter.
   - Contengan algún número.
   Ordenados alfabéticamente, enviando errores a temp/errores9:
   ```bash
   find /home -type f -printf '%f\n' 2> temp/errores9 | grep -E '^[A-Za-z]e.*[0-9]' | sort > varios/nodir
   ```

10. **Añadir al final del fichero nodir la hora del sistema con el formato indicado:**  
    ```bash
    echo "Hola. Son las $(date '+%T'). Esta en $(hostname). Es $(date '+%A, %d de %B de %Y')" >> varios/nodir
    ```

11. **Crear un enlace simbólico en varios llamado enlace que apunte al fichero errores9 (ubicado en temp):**  
    ```bash
    ln -s ../temp/errores9 varios/enlace
    ```

12. **Listar todos los ficheros (incluyendo los ocultos) de la carpeta personal en orden inverso y guardar la salida en listadoL3.txt en varios:**  
    ```bash
    ls -a ~ | sort -r > varios/listadoL3.txt
    ```

13. **Copiar, con una sola orden, todos los ficheros de /home/so/ficheros modificados hace más de dos horas y que tengan un número en el segundo carácter del nombre, al directorio temp:**  
    ```bash
    find /home/so/ficheros -type f -mmin +120 -regex '.{1}[0-9].*' -exec cp {} temp \;
    ```

14. **(Administración) Comprobar que el servidor ssh está activo:**  
    ```bash
    systemctl status ssh
    ```  
    o, según la distribución:
    ```bash
    service ssh status
    ```

15. **(Administración) Crear dos nuevos usuarios us1 y us2 con uids 1100 y 1101:**  
    ```bash
    useradd -u 1100 us1
    useradd -u 1101 us2
    ```

16. **(Administración) Crear un nuevo grupo llamado junio20 con gid 1005:**  
    ```bash
    groupadd -g 1005 junio20
    ```

17. **(Administración) Visualizar página a página los dos ficheros en los que se encuentran las cuentas de los grupos:**  
    (Por ejemplo, /etc/group y /etc/gshadow)
    ```bash
    less /etc/group /etc/gshadow
    ```

18. **(Guión) Crear un script en guion llamado guardar que reciba un parámetro y:**
    ```bash
    #!/bin/bash
    if [ "$#" -ne 1 ]; then
      echo "Error: se requiere un parámetro"
      exit 1
    fi
    read -p "Ingrese un número (1-10): " num
    if [ "$num" -lt 1 ] || [ "$num" -gt 10 ]; then
      echo "Error: número fuera de rango"
      exit 1
    fi
    outfile="../varios/$1"
    > "$outfile"
    for ((i=1; i<=num; i++)); do
      echo "$num" >> "$outfile"
    done
    ```
    Hacerlo ejecutable:
    ```bash
    chmod +x pruebaL3/guion/guardar
    ```

19. **Ejecutar el script guardar pasando el parámetro fichp:**  
    ```bash
    ./guardar fichp
    ```

20. **(Guión) Crear un script en guion llamado ejecut que reciba dos parámetros y:**  
    ```bash
    chmod +x pruebaL3/guion/ejecut
    
