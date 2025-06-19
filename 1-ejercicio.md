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


