# cib07-evidencias-valeria-conejo-ramirez

# Login exitoso
<img width="1006" height="553" alt="image" src="https://github.com/user-attachments/assets/13499e62-a8c3-4dd8-ba24-848aa0e2ab11" />


# Contrasena incorrecta
<img width="928" height="704" alt="image" src="https://github.com/user-attachments/assets/de1b2da7-844e-44b6-9628-51b0aa41a1eb" />


# Usuario inexistente
<img width="937" height="653" alt="image" src="https://github.com/user-attachments/assets/6a353a72-7163-4208-af3e-5f01f2ce2801" />



# Bloqueo por 3 intentos
<img width="833" height="683" alt="image" src="https://github.com/user-attachments/assets/fe70854a-03f7-4fe0-bad7-fa2b85ebd9df" />



# Cerrar sesion
<img width="990" height="687" alt="image" src="https://github.com/user-attachments/assets/af145900-4aae-4a8f-aade-0f3615bed1f1" />

# a) ¿Que es la autenticacion y como la implementa tu programa?
Es un proceso que comprueba la persona que intenta ingresar. El programa lo que realiza es que pregunta el usuario de la persona y su contraseña, y el sistema compara los datos con los datos registrados. Si son correctos, se procede a iniciar sesión automáticamente; en caso de que no, muestra el mensaje de error y permite realizar otros intentos, que usualmente son 3 intentos.


# b) ¿Por que es un problema de seguridad guardar contrasenas en texto plano? ¿Como se resuelve en sistemas reales?
Es un problema si una persona tiene acceso a la base de datos, podría observar de manera fácil los usuarios y contraseñas, logrando que una persona pueda ingresar. En sistemas reales se utilizan funciones de hash, como bcrypt o SHA-256, para proteger las contraseñas y evitar guardarlas directamente.


# c) ¿Que mejora de seguridad le agregarias a este sistema si tuvieras mas tiempo?
Se podría agregar una opción para recuperar la contraseña, en dado caso de que estas se pierdan o se olviden, además de agregar un paso más de autentificación de usuario, para aumentar la seguridad.


# d) ¿En que fase del SDL se deben tomar decisiones sobre el manejo de contrasenas y por que?
Se debe de tomar en la fase del diseño del SDL, dado a que la seguridad debe considerarse desde el inicio del desarrollo del programa y no cuando ya está finalizado. De esta manera se pueden establecer medidas para proteger contraseñas y el acceso desde el diseño.


# Codigo:

# ============================================
# SISTEMA DE LOGIN CON INTERFAZ GRAFICA
# CIB-07 Modulo 7 - Ciberseguridad
# ============================================

import tkinter as tk


# ============================================
# USUARIOS Y CONFIGURACION
# ============================================

usuarios = {
    "admin": "Admin2024!",
    "estudiante1": "Seguridad1!",
    "estudiante2": "Python2024!",
    "invitado": "Invitado1!"
}

MAX_INTENTOS = 3
intentos = [0]


# ============================================
# VERIFICACION DEL LOGIN
# ============================================

def verificar_login():
    usuario = entrada_usuario.get()
    contrasena = entrada_contrasena.get()

    if intentos[0] >= MAX_INTENTOS:
        return

    if usuario in usuarios and usuarios[usuario] == contrasena:
        ventana_login.withdraw()
        abrir_sesion(usuario)
    else:
        intentos[0] += 1
        restantes = MAX_INTENTOS - intentos[0]

        if restantes > 0:
            etiqueta_error.config(
                text=f"Credenciales incorrectas. Intentos restantes: {restantes}"
            )
        else:
            etiqueta_error.config(
                text="ACCESO BLOQUEADO: demasiados intentos fallidos."
            )

            boton_login.config(state="disabled")
            entrada_usuario.config(state="disabled")
            entrada_contrasena.config(state="disabled")


# ============================================
# VENTANA DE SESION ACTIVA
# ============================================

def abrir_sesion(nombre_usuario):

    ventana_sesion = tk.Toplevel()
    ventana_sesion.title("Sesion Activa")
    ventana_sesion.geometry("400x300")
    ventana_sesion.configure(bg="#1e1e2e")
    ventana_sesion.resizable(False, False)

    tk.Label(
        ventana_sesion,
        text=f"Bienvenido/a, {nombre_usuario.upper()}!",
        font=("Arial", 16, "bold"),
        bg="#1e1e2e",
        fg="#a6e3a1"
    ).pack(pady=30)

    tk.Label(
        ventana_sesion,
        text="Sesion iniciada correctamente.",
        font=("Arial", 11),
        bg="#1e1e2e",
        fg="#cdd6f4"
    ).pack()

    frame_info = tk.Frame(
        ventana_sesion,
        bg="#313244",
        padx=20,
        pady=15
    )

    frame_info.pack(pady=20, padx=30, fill="x")

    tk.Label(
        frame_info,
        text=f"Usuario: {nombre_usuario}",
        font=("Arial", 10),
        bg="#313244",
        fg="#cdd6f4",
        anchor="w"
    ).pack(fill="x")

    tk.Label(
        frame_info,
        text="Rol: Usuario estandar",
        font=("Arial", 10),
        bg="#313244",
        fg="#cdd6f4",
        anchor="w"
    ).pack(fill="x")

    tk.Label(
        frame_info,
        text="Estado: Activo",
        font=("Arial", 10),
        bg="#313244",
        fg="#a6e3a1",
        anchor="w"
    ).pack(fill="x")


    # ========================================
    # FUNCION PARA CERRAR SESION
    # ========================================

    def cerrar_sesion():
        ventana_sesion.destroy()
        ventana_login.deiconify()

        entrada_usuario.delete(0, tk.END)
        entrada_contrasena.delete(0, tk.END)

        etiqueta_error.config(text="")

        intentos[0] = 0

        boton_login.config(state="normal")
        entrada_usuario.config(state="normal")
        entrada_contrasena.config(state="normal")


    # Boton para cerrar la sesion
    tk.Button(
        ventana_sesion,
        text="Cerrar Sesion",
        command=cerrar_sesion,
        bg="#f38ba8",
        fg="white",
        font=("Arial", 11, "bold"),
        padx=20,
        pady=8,
        bd=0,
        cursor="hand2"
    ).pack(pady=10)


# ============================================
# VENTANA PRINCIPAL
# ============================================

ventana_login = tk.Tk()
ventana_login.title("Sistema de Login - CIB-07")
ventana_login.geometry("420x480")
ventana_login.configure(bg="#1e1e2e")
ventana_login.resizable(False, False)


# ============================================
# ENCABEZADO
# ============================================

frame_header = tk.Frame(
    ventana_login,
    bg="#313244",
    pady=20
)

frame_header.pack(fill="x")

tk.Label(
    frame_header,
    text="SISTEMA DE LOGIN",
    font=("Arial", 18, "bold"),
    bg="#313244",
    fg="#cba6f7"
).pack()

tk.Label(
    frame_header,
    text="CIB-07 Modulo 7 - Ciberseguridad",
    font=("Arial", 10),
    bg="#313244",
    fg="#6c7086"
).pack()


# ============================================
# FORMULARIO DE LOGIN
# ============================================

frame_form = tk.Frame(
    ventana_login,
    bg="#1e1e2e",
    padx=40,
    pady=30
)

frame_form.pack(fill="both", expand=True)

tk.Label(
    frame_form,
    text="Nombre de usuario",
    font=("Arial", 11),
    bg="#1e1e2e",
    fg="#cdd6f4",
    anchor="w"
).pack(fill="x")

entrada_usuario = tk.Entry(
    frame_form,
    font=("Arial", 12),
    bg="#313244",
    fg="#cdd6f4",
    insertbackground="white",
    bd=0,
    highlightthickness=1,
    highlightcolor="#cba6f7",
    highlightbackground="#45475a"
)

entrada_usuario.pack(fill="x", ipady=8, pady=(4, 16))

tk.Label(
    frame_form,
    text="Contrasena",
    font=("Arial", 11),
    bg="#1e1e2e",
    fg="#cdd6f4",
    anchor="w"
).pack(fill="x")

entrada_contrasena = tk.Entry(
    frame_form,
    font=("Arial", 12),
    bg="#313244",
    fg="#cdd6f4",
    insertbackground="white",
    bd=0,
    show="*",
    highlightthickness=1,
    highlightcolor="#cba6f7",
    highlightbackground="#45475a"
)

entrada_contrasena.pack(fill="x", ipady=8, pady=(4, 24))

boton_login = tk.Button(
    frame_form,
    text="Iniciar Sesion",
    command=verificar_login,
    bg="#cba6f7",
    fg="#1e1e2e",
    font=("Arial", 12, "bold"),
    padx=20,
    pady=10,
    bd=0,
    cursor="hand2"
)

boton_login.pack(fill="x")

etiqueta_error = tk.Label(
    frame_form,
    text="",
    font=("Arial", 10),
    bg="#1e1e2e",
    fg="#f38ba8",
    wraplength=320
)

etiqueta_error.pack(pady=(16, 0))


# ============================================
# PIE DE LA VENTANA
# ============================================

frame_pie = tk.Frame(
    ventana_login,
    bg="#181825",
    pady=12
)

frame_pie.pack(fill="x", side="bottom")

tk.Label(
    frame_pie,
    text="Usuarios de prueba: admin / Admin2024! | invitado / Invitado1!",
    font=("Arial", 8),
    bg="#181825",
    fg="#45475a"
).pack()


# ============================================
# INICIO DEL PROGRAMA
# ============================================

ventana_login.bind("<Return>", lambda e: verificar_login())

ventana_login.mainloop()
