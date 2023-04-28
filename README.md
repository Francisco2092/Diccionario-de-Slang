import sqlite3
import sys

con = sqlite3.connect('slang_panameno.db')
cur = con.cursor()

cur.execute('''CREATE TABLE IF NOT EXISTS palabras
               (id INTEGER PRIMARY KEY AUTOINCREMENT,
                palabra TEXT NOT NULL,
                significado TEXT NOT NULL);''')

def agregar_palabra():
    palabra = input("Ingresa la palabra: ")
    significado = input("Ingresa el significado: ")
    cur.execute("INSERT INTO palabras (palabra, significado) VALUES (?, ?)", (palabra, significado))
    con.commit()

def editar_palabra():
    palabra = input("Ingresa la palabra que deseas editar: ")
    nueva_palabra = input("Ingresa la nueva palabra: ")
    nuevo_significado = input("Ingresa el nuevo significado: ")
    cur.execute("UPDATE palabras SET palabra = ?, significado = ? WHERE palabra = ?", (nueva_palabra, nuevo_significado, palabra))
    con.commit()

def eliminar_palabra():
    palabra = input("Ingresa la palabra que deseas eliminar: ")
    cur.execute("DELETE FROM palabras WHERE palabra = ?", (palabra,))
    con.commit()

def ver_palabras():
    cur.execute("SELECT palabra, significado FROM palabras")
    filas = cur.fetchall()
    for fila in filas:
        print(f"{fila[0]}: {fila[1]}")

def buscar_palabra():
    palabra = input("Ingresa la palabra que deseas buscar: ")
    cur.execute("SELECT significado FROM palabras WHERE palabra = ?", (palabra,))
    fila = cur.fetchone()
    if fila:
        print(fila[0])
    else:
        print("La palabra no fue encontrada en el diccionario.")

def menu():
    while True:
        print("Seleccione una opción:")
        print("a) Agregar nueva palabra")
        print("b) Editar palabra existente")
        print("c) Eliminar palabra existente")
        print("d) Ver listado de palabras")
        print("e) Buscar significado de palabra")
        print("f) Salir")
        opcion = input()
        if opcion == "a":
            agregar_palabra()
        elif opcion == "b":
            editar_palabra()
        elif opcion == "c":
            eliminar_palabra()
        elif opcion == "d":
            ver_palabras()
        elif opcion == "e":
            buscar_palabra()
        elif opcion == "f":
            break
        else:
            print("Opción inválida. Intente nuevamente.")
menu()
