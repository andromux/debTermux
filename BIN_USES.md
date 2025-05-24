# USO DE BINARIOS

# Guía de Uso - Andromux Remove

**Andromux Remove** es una herramienta de línea de comandos para eliminar repositorios de GitHub de manera segura con confirmación interactiva.

## 🚀 Instalación y Configuración

### Prerrequisitos
```bash
# Instalar GitHub CLI si no lo tienes
pkg install -y gh

# Autenticarse en GitHub CLI
gh auth login

# Hacer el script ejecutable
chmod +x andromux-remove

# Opcional: mover a PATH para uso global
mv andromux-remove $PREFIX/bin/
```

## 📋 Sintaxis

```bash
andromux-remove <nombre_del_repositorio>
```

## 🔍 Ejemplos de Uso

### 1. Uso Básico - Eliminar un Repositorio

#### Comando:
```bash
andromux-remove mi-proyecto-viejo
```

#### Salida esperada:
```bash
¿Estás seguro de que deseas eliminar 'mi-proyecto-viejo'? (s/n): s
Eliminando repositorio 'mi-proyecto-viejo'...
✓ Deleted repository andromux/mi-proyecto-viejo
```

### 2. Cancelar la Eliminación

#### Comando:
```bash
andromux-remove proyecto-importante
```

#### Salida esperada:
```bash
¿Estás seguro de que deseas eliminar 'proyecto-importante'? (s/n): n
Cancelado.
```

#### También funciona con:
```bash
¿Estás seguro de que deseas eliminar 'proyecto-importante'? (s/n): no
Cancelado.
```

### 3. Diferentes Respuestas de Confirmación

#### Respuestas que ELIMINAN el repositorio:
```bash
# Respuestas válidas para confirmar
¿Estás seguro de que deseas eliminar 'test-repo'? (s/n): s
¿Estás seguro de que deseas eliminar 'test-repo'? (s/n): S
¿Estás seguro de que deseas eliminar 'test-repo'? (s/n): si
¿Estás seguro de que deseas eliminar 'test-repo'? (s/n): SI
```

#### Respuestas que CANCELAN la eliminación:
```bash
# Cualquier otra respuesta cancela
¿Estás seguro de que deseas eliminar 'test-repo'? (s/n): n
¿Estás seguro de que deseas eliminar 'test-repo'? (s/n): no
¿Estás seguro de que deseas eliminar 'test-repo'? (s/n): N
¿Estás seguro de que deseas eliminar 'test-repo'? (s/n): [Enter]
¿Estás seguro de que deseas eliminar 'test-repo'? (s/n): cualquier-cosa
```

## ⚠️ Casos de Error

### 1. Sin Argumentos

#### Comando:
```bash
andromux-remove
```

#### Salida de error:
```bash
Uso: andromux-remove <nombre_del_repositorio>
```

### 2. Repositorio No Existe

#### Comando:
```bash
andromux-remove repo-inexistente
```

#### Salida esperada:
```bash
¿Estás seguro de que deseas eliminar 'repo-inexistente'? (s/n): s
Eliminando repositorio 'repo-inexistente'...
X Could not resolve to a Repository with the name 'andromux/repo-inexistente'.
```

### 3. Sin Permisos

#### Comando:
```bash
andromux-remove repo-sin-permisos
```

#### Salida esperada:
```bash
¿Estás seguro de que deseas eliminar 'repo-sin-permisos'? (s/n): s
Eliminando repositorio 'repo-sin-permisos'...
X You don't have permission to delete this repository
```

### 4. GitHub CLI No Autenticado

#### Comando:
```bash
andromux-remove mi-repo
```

#### Salida de error:
```bash
¿Estás seguro de que deseas eliminar 'mi-repo'? (s/n): s
Eliminando repositorio 'mi-repo'...
error: authentication required
To authenticate, run: gh auth login
```

## 🎯 Ejemplos Prácticos por Escenario

### Escenario 1: Limpiar Repositorios de Prueba

```bash
# Eliminar múltiples repos de prueba
andromux-remove test-1
# ¿Estás seguro de que deseas eliminar 'test-1'? (s/n): s
# Eliminando repositorio 'test-1'...
# ✓ Deleted repository andromux/test-1

andromux-remove test-2
# ¿Estás seguro de que deseas eliminar 'test-2'? (s/n): s
# Eliminando repositorio 'test-2'...
# ✓ Deleted repository andromux/test-2

andromux-remove test-3
# ¿Estás seguro de que deseas eliminar 'test-3'? (s/n): n
# Cancelado.
```

### Escenario 2: Eliminar Proyecto Obsoleto

```bash
andromux-remove proyecto-descontinuado
```
**Interacción completa:**
```
¿Estás seguro de que deseas eliminar 'proyecto-descontinuado'? (s/n): s
Eliminando repositorio 'proyecto-descontinuado'...
✓ Deleted repository andromux/proyecto-descontinuado
```

### Escenario 3: Accidentalmente Intentar Eliminar Repo Importante

```bash
andromux-remove mi-proyecto-principal
```
**Interacción completa:**
```
¿Estás seguro de que deseas eliminar 'mi-proyecto-principal'? (s/n): n
Cancelado.
```

## 🔒 Medidas de Seguridad

### 1. Confirmación Obligatoria
- **SIEMPRE** pide confirmación antes de eliminar
- Solo acepta `s` o `S` (y variantes) como confirmación
- Cualquier otra respuesta cancela la operación

### 2. Nombres Explícitos
- Muestra el nombre exacto del repositorio a eliminar
- No permite wildcards o eliminación masiva

### 3. Usuario Hardcodeado
- Solo elimina repositorios del usuario `andromux`
- Previene eliminación accidental de repos de otros usuarios

## 📝 Flujo de Trabajo Recomendado

### Antes de Eliminar:
1. **Listar repositorios** para verificar el nombre exacto:
```bash
andromux -l | grep nombre-parcial
```

2. **Verificar contenido importante**:
```bash
andromux -o nombre-repo  # Abrir en navegador para revisar
```

3. **Hacer backup si es necesario**:
```bash
andromux nombre-repo  # Clonar antes de eliminar
```

### Durante la Eliminación:
```bash
andromux-remove nombre-repo
# Leer cuidadosamente el nombre mostrado
# Confirmar solo si estás 100% seguro
```

## 🛠️ Combinación con Otros Comandos

### Ejemplo de Flujo Completo:
```bash
# 1. Listar repositorios para encontrar cuáles eliminar
andromux -l -n 50

# 2. Ver detalles de un repo específico
andromux -o repo-candidato

# 3. Clonar backup si tiene algo importante
andromux repo-candidato

# 4. Eliminar el repositorio
andromux-remove repo-candidato
```

## 🚨 Advertencias Importantes

### ⚠️ **ELIMINACIÓN PERMANENTE**
- Una vez eliminado, **NO SE PUEDE RECUPERAR**
- GitHub no tiene "papelera de reciclaje" para repositorios
- Asegúrate de tener backups de código importante

### ⚠️ **Verificar Nombre**
- El script no valida si el repositorio existe antes de preguntar
- Lee cuidadosamente el nombre en la confirmación
- Un typo puede intentar eliminar un repo que no existe

### ⚠️ **Dependencias Externas**
- Requiere `gh` (GitHub CLI) instalado y autenticado
- Necesita permisos de eliminación en el repositorio

## 🐛 Códigos de Salida

- **0**: Operación exitosa (eliminado o cancelado correctamente)
- **1**: Error de uso (sin argumentos)
- **Otros**: Errores de GitHub CLI (repo no existe, sin permisos, etc.)

## 📋 Lista de Verificación Pre-Eliminación

Antes de usar `andromux-remove`, verifica:

- [ ] ¿Es realmente el repositorio correcto?
- [ ] ¿Hay código importante que necesito respaldar?
- [ ] ¿Hay issues o pull requests importantes?
- [ ] ¿Otros desarrolladores dependen de este repo?
- [ ] ¿Hay documentación o wiki que perderé?
- [ ] ¿Estoy 100% seguro de eliminarlo?

## 🔄 Alternativas Antes de Eliminar

En lugar de eliminar, considera:

1. **Archivar el repositorio**:
```bash
gh repo archive andromux/nombre-repo
```

2. **Hacer privado**:
```bash
gh repo edit andromux/nombre-repo --visibility private
```

3. **Renombrar para marcarlo como obsoleto**:
```bash
gh repo rename andromux/nombre-repo obsoleto-nombre-repo
```

# andromux-releases

## 🚀 Mejoras implementadas:

### **1. Interfaz profesional**
- Sistema de colores consistente
- Logs estructurados con niveles
- Ayuda detallada con ejemplos
- Modo silencioso y dry-run

### **2. Funcionalidades avanzadas**
- **Auto-incremento de versión**: `-a` incrementa automáticamente desde la última versión
- **Changelog automático**: `-c` genera changelog con commits y estadísticas
- **Gestión completa**: Listar, eliminar y actualizar releases
- **Validación de archivos**: Soporte para múltiples formatos
- **Pre-releases y borradores**: Opciones `-p` y `-D`

### **3. Robustez y seguridad**
- Modo estricto bash (`set -euo pipefail`)
- Validación completa de dependencias
- Verificación de autenticación GitHub
- Confirmaciones de seguridad para operaciones destructivas
- Manejo de errores mejorado

### **4. Características empresariales**
- Configuración flexible por parámetros
- Información detallada del archivo (tamaño, MD5)
- URLs directas a releases creados
- Soporte para diferentes owners/repos
- Modo `--force` para automatización

## 📖 Ejemplos de uso:

```bash
# Uso básico mejorado
./release-manager app.apk

# Auto-incrementar versión con changelog
./release-manager -a -c app.apk

# Pre-release con título personalizado
./release-manager -p -t "Beta v2.1.0" -f app-beta.apk

# Listar releases existentes
./release-manager --list

# Eliminar un release
./release-manager --delete v1.0.0

# Modo dry-run (ver qué haría sin ejecutar)
./release-manager -n -a -c app.apk
```

## 🔧 Nuevas capacidades:

1. **Gestión completa del ciclo de vida** de releases
2. **Automatización**: Auto-incremento y changelog
3. **Validación robusta** de archivos y versiones
4. **Modo interactivo mejorado** con confirmaciones
5. **Logging profesional** con colores y niveles
6. **Compatibilidad extendida** con múltiples formatos de archivo


# andromux

**Andromux** es una herramienta de línea de comandos para gestionar repositorios de GitHub de manera eficiente desde Termux o cualquier terminal Linux.

## 📋 Sintaxis Básica

```bash
andromux [opciones] <nombre_del_repositorio>
```

## 🔍 Ejemplos de Uso

### 1. Mostrar Ayuda
```bash
andromux --help
andromux -h
```
**Salida esperada:**
```
Andromux - CLI para gestionar repositorios GitHub

Uso: andromux [opciones] <nombre_del_repositorio>

Opciones de listado:
  -l, --list             Listar repositorios
  -p, --public           Solo públicos
  -c, --close            Solo privados
  -n, --numero <N>       Número de repos a mostrar (por defecto: 30)

Opciones de repositorio:
  -o, --open <repo>      Abrir repositorio en el navegador

Otras opciones:
  -h, --help             Mostrar esta ayuda

Ejemplos:
  andromux --list
  andromux -l -p -n 20
  andromux -o mi-repo
  andromux mi-repo
```

### 2. Listar Repositorios

#### Listar todos los repositorios (máximo 30)
```bash
andromux --list
andromux -l
```
**Salida esperada:**
```
Listando repositorios (all) de andromux (máx 30)...

mi-proyecto-web [PUBLIC] - 2024-01-15
  Aplicación web moderna con React y Node.js

scripts-termux [PRIVATE] - 2024-01-10
  Colección de scripts útiles para Termux

dotfiles [PUBLIC] - 2024-01-08
  Configuraciones personalizadas para desarrollo
```

#### Listar solo repositorios públicos
```bash
andromux -l -p
andromux --list --public
```
**Salida esperada:**
```
Listando repositorios (public) de andromux (máx 30)...

mi-proyecto-web [PUBLIC] - 2024-01-15
  Aplicación web moderna con React y Node.js

dotfiles [PUBLIC] - 2024-01-08
  Configuraciones personalizadas para desarrollo

open-source-tool [PUBLIC] - 2024-01-05
  Herramienta de código abierto para desarrolladores
```

#### Listar solo repositorios privados
```bash
andromux -l -c
andromux --list --close
```
**Salida esperada:**
```
Listando repositorios (private) de andromux (máx 30)...

scripts-termux [PRIVATE] - 2024-01-10
  Colección de scripts útiles para Termux

proyecto-personal [PRIVATE] - 2024-01-03
  Proyecto de desarrollo personal

config-backup [PRIVATE] - 2023-12-28
```

#### Limitar número de repositorios mostrados
```bash
andromux -l -n 10
andromux --list --numero 5
andromux -l -p -n 15  # Solo públicos, máximo 15
```
**Salida esperada:**
```
Listando repositorios (all) de andromux (máx 10)...

mi-proyecto-web [PUBLIC] - 2024-01-15
  Aplicación web moderna con React y Node.js

scripts-termux [PRIVATE] - 2024-01-10
  Colección de scripts útiles para Termux

dotfiles [PUBLIC] - 2024-01-08
  Configuraciones personalizadas para desarrollo

[... hasta 10 repositorios máximo]
```

### 3. Abrir Repositorios en el Navegador

#### Abrir un repositorio específico
```bash
andromux -o mi-proyecto-web
andromux --open dotfiles
```
**Salida esperada:**
```
Abriendo repositorio: andromux/mi-proyecto-web
URL: https://github.com/andromux/mi-proyecto-web
✓ Repositorio abierto en el navegador
```

#### Si el repositorio no existe
```bash
andromux -o repo-inexistente
```
**Salida esperada:**
```
Abriendo repositorio: andromux/repo-inexistente
✗ Error: El repositorio 'andromux/repo-inexistente' no existe o no tienes acceso
```

### 4. Clonar Repositorios

#### Clonar un repositorio
```bash
andromux mi-proyecto-web
andromux dotfiles
```
**Salida esperada:**
```
Clonando repositorio: mi-proyecto-web
Cloning into 'mi-proyecto-web'...
remote: Enumerating objects: 125, done.
remote: Counting objects: 100% (125/125), done.
remote: Compressing objects: 100% (89/89), done.
remote: Total 125 (delta 45), reused 98 (delta 32), pack-reused 0
Receiving objects: 100% (125/125), 2.4 MiB | 1.8 MiB/s, done.
Resolving deltas: 100% (45/45), done.
✓ Repositorio clonado exitosamente
```

## ⚠️ Mensajes de Error Comunes

### 1. GitHub CLI no autenticado
```bash
andromux -l
```
**Error:**
```
[Error] No estás autenticado en GitHub CLI
Ejecuta: gh auth login
```

### 2. Dependencias faltantes
```bash
andromux -l
```
**Si falta jq:**
```
[Error] jq no está instalado. Instalando...
```
**Si falta GitHub CLI:**
```
[Error] GitHub CLI no está instalado. Instalando...
```

### 3. Sin argumentos válidos
```bash
andromux
```
**Error:**
```
[Error] Debes especificar una acción o nombre de repositorio
Usa andromux --help para ver las opciones disponibles
```

### 4. Repositorio sin especificar para abrir
```bash
andromux -o
```
**Error:**
```
[Error] Debes especificar el nombre del repositorio
Ejemplo: andromux -o mi-repo
```

## 🔄 Combinaciones de Comandos Útiles

### Flujo de trabajo típico
```bash
# 1. Ver repositorios públicos recientes
andromux -l -p -n 10

# 2. Clonar un repositorio específico
andromux nombre-del-repo

# 3. Abrir el repositorio en GitHub para ver issues
andromux -o nombre-del-repo
```

### Exploración de repositorios
```bash
# Ver todos los repositorios
andromux -l

# Ver solo los primeros 5 repositorios públicos
andromux -l -p -n 5

# Ver repositorios privados
andromux -l -c
```

## 🎨 Colores en la Salida

El script utiliza colores para mejorar la legibilidad:
- **🔵 Azul**: Información general y nombres de repositorios
- **🟢 Verde**: Repositorios públicos y mensajes de éxito  
- **🟡 Amarillo**: Repositorios privados y advertencias
- **🔴 Rojo**: Errores
- **🟦 Cian**: Títulos y URLs

## 📱 Compatibilidad

- **Termux (Android)**: Completamente compatible
- **Linux**: Compatible con la mayoría de distribuciones
- **macOS**: Compatible (con homebrew)
- **Windows WSL**: Compatible

## 🛠️ Solución de Problemas

1. **Si el comando no se encuentra**: Asegúrate de que el script esté en tu PATH o usa la ruta completa
2. **Si no se abren los enlaces**: El script intentará usar `termux-open-url`, `xdg-open`, o `open` según tu sistema
3. **Si falla la autenticación**: Ejecuta `gh auth status` para verificar tu estado de autenticación

## 📝 Notas Adicionales

- El script busca repositorios del usuario configurado como `GITHUB_USER="andromux"`
- Por defecto muestra máximo 30 repositorios
- Los repositorios se ordenan por fecha de última actualización
- Las descripciones vacías no se muestran
