# 🧪 Laboratorio: “Zona Restringida para Recursos Legales”

---

## 🎯 Objetivo del Estudiante

* Crear una estructura aislada para un área sensible llamada **Legales**.
* Configurar usuarios y grupos con acceso exclusivo a una carpeta.
* Aplicar una GPO que **bloquee el acceso al Panel de Control**.
* Validar el acceso con evidencias visuales claras.
* Aprender a controlar la herencia, permisos, aplicación de políticas y acceso compartido por red.

---

## 🖥️ Infraestructura Necesaria

| Máquina       | Rol                          | Sistema Operativo        |
| ------------- | ---------------------------- | ------------------------ |
| `SRV-LEGAL01` | Controlador de Dominio + DNS | Windows Server 2019/2022 |
| `PC-LEGAL01`  | Cliente unido al dominio     | Windows 10/11 Pro        |

---

## 📜 Enunciado

> Eres el administrador de TI de **JustoCorp**, una empresa que debe resguardar el acceso a documentos del área legal.
> Debes configurar un entorno donde:
>
> * Solo los miembros del área `Legales` tengan acceso a los documentos compartidos.
> * El usuario asignado **no pueda acceder al Panel de control**.
> * El equipo `PC-LEGAL01` se una al dominio y se valide el resultado.

---

## 🪜 Solución paso a paso

---

### 🔹 Paso 1: Crear el dominio `justocorp.local`

1. En `SRV-LEGAL01`, abre **Administrador del servidor**.
2. Agrega los roles:

   * **Active Directory Domain Services (AD DS)**
   * **DNS Server**
3. Promociona a controlador de dominio:

   * Nuevo bosque: `justocorp.local`
   * Establece una contraseña DSRM segura
4. Reinicia al finalizar.

---

### 🔹 Paso 2: Crear OU, usuario y grupo

1. Abre `dsa.msc`.
2. Crea una nueva **Unidad Organizativa (OU)**:

   * Nombre: `Legales`
3. Dentro de la OU `Legales`:

   * Usuario:

     * Nombre: `LauraPerez`
     * Usuario: `lperez`
   * Grupo: `Grupo_Legales`
4. Agrega `lperez` al grupo `Grupo_Legales`:

   * En el usuario > pestaña **Miembro de** > **Agregar** > `Grupo_Legales`

---

### 🔹 Paso 3: Crear carpeta compartida con permisos

1. En `SRV-LEGAL01`, crea:

   ```
   C:\Areas\Legales
   ```

2. Click derecho > **Propiedades** > pestaña **Seguridad** > **Opciones avanzadas**

   * Desactiva herencia
   * Quita entradas genéricas como `Usuarios`
   * Agrega:

     * `Grupo_Legales` con **Control total**
     * (Opcional) `Administradores` y `SYSTEM`

3. Compartir:

   * Pestaña **Compartir** > Uso compartido avanzado > Compartir como: `Legales`
   * Permisos: solo `Grupo_Legales` con **Control total**

---

### 🔹 Paso 4: Crear GPO para bloquear el Panel de Control

1. Abre `gpmc.msc`.
2. Click derecho sobre la OU `Legales` > **Crear GPO y vincular aquí**:

   * Nombre: `BloqueoPanelControl`
3. Edita la GPO:

   Ruta:

   ```
   Configuración de usuario >
   Plantillas administrativas >
   Panel de control >
   Impedir el acceso al Panel de control y a Configuración del PC
   ```

   * Activa la opción: **Habilitada**

---

### 🔹 Paso 5: Unir `PC-LEGAL01` al dominio

1. En el equipo cliente:

   * Asigna IP fija y como DNS la IP de `SRV-LEGAL01`
   * Nombre de equipo: `PC-LEGAL01`
   * Unir al dominio: `justocorp.local`
   * Reinicia al finalizar

---

### 🔹 Paso 6: Validar desde el cliente

1. Inicia sesión en `PC-LEGAL01` con:

   ```
   Usuario: lperez
   Dominio: justocorp.local
   ```

2. Verifica:

   * ✅ Puede acceder a:

     ```
     \\SRV-LEGAL01\Legales
     ```

     y crear archivos

   * ❌ Intenta abrir **Panel de control** → recibe mensaje de bloqueo

---

## 📸 Evidencias a entregar

1. Captura del dominio `justocorp.local`
2. OU `Legales` con el usuario y grupo correctamente creados
3. Configuración de permisos NTFS y compartición
4. GPO editada y vinculada
5. Acceso exitoso desde cliente
6. Bloqueo efectivo del Panel de control

---

## 💪 BONUS PowerShell

En `SRV-LEGAL01`, mostrar los miembros del grupo:

```powershell
Get-ADGroupMember -Identity "Grupo_Legales"
```

---

## 🧠 Aprendizajes esperados

* Creación y vinculación de GPOs prácticas y visibles
* Asignación de permisos de forma segura (romper herencia)
* Acceso controlado a recursos mediante grupos
* Validación funcional en cliente final

