import tkinter as tk
from tkinter import messagebox
import json
import os

class Candidato:
    def __init__(self, nombre, experiencia, habilidades):
        self.nombre = nombre
        self.experiencia = experiencia
        self.habilidades = habilidades

    def __str__(self):
        return f"Nombre: {self.nombre}, Experiencia: {self.experiencia} años, Habilidades: {', '.join(self.habilidades)}"

class SistemaReclutamiento:
    def __init__(self, archivo='candidatos.json'):
        self.archivo = archivo
        self.candidatos = self.cargar_candidatos()

    def cargar_candidatos(self):
        if os.path.exists(self.archivo):
            with open(self.archivo, 'r') as file:
                return json.load(file)
        return []

    def guardar_candidatos(self):
        with open(self.archivo, 'w') as file:
            json.dump(self.candidatos, file, indent=4)

    def agregar_candidato(self, candidato):
        self.candidatos.append(candidato.__dict__)
        self.guardar_candidatos()

    def listar_candidatos(self):
        return [Candidato(**candidato) for candidato in self.candidatos]

    def buscar_candidato(self, nombre):
        for candidato in self.candidatos:
            if candidato['nombre'].lower() == nombre.lower():
                return Candidato(**candidato)
        return None

    def eliminar_candidato(self, nombre):
        for i, candidato in enumerate(self.candidatos):
            if candidato['nombre'].lower() == nombre.lower():
                del self.candidatos[i]
                self.guardar_candidatos()
                return True
        return False

class App:
    def __init__(self, root):
        self.root = root
        self.root.title("Sistema de Reclutamiento")

        self.sistema_reclutamiento = SistemaReclutamiento()

        # Frames
        self.frame = tk.Frame(root)
        self.frame.pack(pady=10)

        # Entradas
        self.nombre_label = tk.Label(self.frame, text="Nombre")
        self.nombre_label.grid(row=0, column=0)
        self.nombre_entry = tk.Entry(self.frame)
        self.nombre_entry.grid(row=0, column=1)

        self.experiencia_label = tk.Label(self.frame, text="Años de Experiencia")
        self.experiencia_label.grid(row=1, column=0)
        self.experiencia_entry = tk.Entry(self.frame)
        self.experiencia_entry.grid(row=1, column=1)

        self.habilidades_label = tk.Label(self.frame, text="Habilidades (separadas por coma)")
        self.habilidades_label.grid(row=2, column=0)
        self.habilidades_entry = tk.Entry(self.frame)
        self.habilidades_entry.grid(row=2, column=1)

        # Botones
        self.agregar_button = tk.Button(self.frame, text="Agregar Candidato", command=self.agregar_candidato)
        self.agregar_button.grid(row=3, columnspan=2, pady=10)

        self.listar_button = tk.Button(self.frame, text="Listar Candidatos", command=self.listar_candidatos)
        self.listar_button.grid(row=4, columnspan=2, pady=10)

        self.buscar_button = tk.Button(self.frame, text="Buscar Candidato", command=self.buscar_candidato)
        self.buscar_button.grid(row=5, columnspan=2, pady=10)

        self.eliminar_button = tk.Button(self.frame, text="Eliminar Candidato", command=self.eliminar_candidato)
        self.eliminar_button.grid(row=6, columnspan=2, pady=10)

    def agregar_candidato(self):
        nombre = self.nombre_entry.get()
        experiencia = self.experiencia_entry.get()
        habilidades = self.habilidades_entry.get().split(',')

        if not nombre or not experiencia or not habilidades:
            messagebox.showerror("Error", "Todos los campos son obligatorios.")
            return

        try:
            experiencia = int(experiencia)
        except ValueError:
            messagebox.showerror("Error", "La experiencia debe ser un número.")
            return

        candidato = Candidato(nombre, experiencia, [habilidad.strip() for habilidad in habilidades])
        self.sistema_reclutamiento.agregar_candidato(candidato)
        messagebox.showinfo("Éxito", "Candidato agregado exitosamente.")
        self.limpiar_campos()

    def listar_candidatos(self):
        candidatos = self.sistema_reclutamiento.listar_candidatos()
        if not candidatos:
            messagebox.showinfo("Información", "No hay candidatos registrados.")
        else:
            lista = "\n".join([str(candidato) for candidato in candidatos])
            messagebox.showinfo("Lista de Candidatos", lista)

    def buscar_candidato(self):
        nombre = self.nombre_entry.get()
        candidato = self.sistema_reclutamiento.buscar_candidato(nombre)
        if candidato:
            messagebox.showinfo("Candidato Encontrado", str(candidato))
        else:
            messagebox.showinfo("Información", "Candidato no encontrado.")

    def eliminar_candidato(self):
        nombre = self.nombre_entry.get()
        if self.sistema_reclutamiento.eliminar_candidato(nombre):
            messagebox.showinfo("Éxito", "Candidato eliminado exitosamente.")
        else:
            messagebox.showinfo("Información", "Candidato no encontrado.")

    def limpiar_campos(self):
        self.nombre_entry.delete(0, tk.END)
        self.experiencia_entry.delete(0, tk.END)
        self.habilidades_entry.delete(0, tk.END)

if __name__ == "__main__":
    root = tk.Tk()
    app = App(root)
    root.mainloop()
