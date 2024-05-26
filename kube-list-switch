import os
import subprocess
import tkinter as tk
from tkinter import messagebox, scrolledtext

def get_kube_contexts():
    result = subprocess.run(['kubectl', 'config', 'get-contexts', '-o', 'name'], stdout=subprocess.PIPE)
    contexts = result.stdout.decode('utf-8').splitlines()
    return contexts

def set_kube_context(context):
    result = subprocess.run(['kubectl', 'config', 'use-context', context], stdout=subprocess.PIPE)
    if result.returncode == 0:
        messagebox.showinfo("Success", f"Context switched to: {context}")
        list_pods()
        list_services()
        list_deployments()
        list_statefulsets()
        list_daemonsets()
        list_ingresses()
    else:
        messagebox.showerror("Error", "Failed to switch context")

def list_pods():
    result = subprocess.run(['kubectl', 'get', 'pods', '--all-namespaces', '-o', 'wide'], stdout=subprocess.PIPE)
    pods = result.stdout.decode('utf-8')
    pod_list_text.delete(1.0, tk.END)
    pod_list_text.insert(tk.END, pods)

def list_services():
    result = subprocess.run(['kubectl', 'get', 'services', '--all-namespaces', '-o', 'wide'], stdout=subprocess.PIPE)
    services = result.stdout.decode('utf-8')
    service_list_text.delete(1.0, tk.END)
    service_list_text.insert(tk.END, services)

def list_deployments():
    result = subprocess.run(['kubectl', 'get', 'deployments', '--all-namespaces', '-o', 'wide'], stdout=subprocess.PIPE)
    deployments = result.stdout.decode('utf-8')
    deployment_list_text.delete(1.0, tk.END)
    deployment_list_text.insert(tk.END, deployments)

def list_statefulsets():
    result = subprocess.run(['kubectl', 'get', 'statefulsets', '--all-namespaces', '-o', 'wide'], stdout=subprocess.PIPE)
    statefulsets = result.stdout.decode('utf-8')
    statefulset_list_text.delete(1.0, tk.END)
    statefulset_list_text.insert(tk.END, statefulsets)

def list_daemonsets():
    result = subprocess.run(['kubectl', 'get', 'daemonsets', '--all-namespaces', '-o', 'wide'], stdout=subprocess.PIPE)
    daemonsets = result.stdout.decode('utf-8')
    daemonset_list_text.delete(1.0, tk.END)
    daemonset_list_text.insert(tk.END, daemonsets)

def list_ingresses():
    result = subprocess.run(['kubectl', 'get', 'ingresses', '--all-namespaces', '-o', 'wide'], stdout=subprocess.PIPE)
    ingresses = result.stdout.decode('utf-8')
    ingress_list_text.delete(1.0, tk.END)
    ingress_list_text.insert(tk.END, ingresses)

def on_select(event):
    selected_context = context_listbox.get(context_listbox.curselection())
    set_kube_context(selected_context)

def search_object():
    search_term = search_entry.get()
    result = subprocess.run(['kubectl', 'get', 'all', '--all-namespaces', '-o', 'wide'], stdout=subprocess.PIPE)
    output = result.stdout.decode('utf-8')
    search_result_text.delete(1.0, tk.END)
    if search_term in output:
        lines = output.split('\n')
        for line in lines:
            if search_term in line:
                search_result_text.insert(tk.END, line + '\n')
    else:
        search_result_text.insert(tk.END, f'No results found for: {search_term}')

app = tk.Tk()
app.title("Kubernetes Context Switcher, Pod and Service Lister")

# Create a canvas and a vertical scrollbar
canvas = tk.Canvas(app)
scrollbar = tk.Scrollbar(app, orient="vertical", command=canvas.yview)
scrollable_frame = tk.Frame(canvas)

scrollable_frame.bind(
    "<Configure>",
    lambda e: canvas.configure(
        scrollregion=canvas.bbox("all")
    )
)

canvas.create_window((0, 0), window=scrollable_frame, anchor="nw")
canvas.configure(yscrollcommand=scrollbar.set)

# Add the scrollbar and canvas to the main window
scrollbar.pack(side="right", fill="y")
canvas.pack(side="left", fill="both", expand=True)

# Context selection section
tk.Label(scrollable_frame, text="Select Kubernetes Context:").pack(pady=10)

context_listbox = tk.Listbox(scrollable_frame, selectmode=tk.SINGLE)
context_listbox.pack(pady=10, padx=10)

contexts = get_kube_contexts()
for context in contexts:
    context_listbox.insert(tk.END, context)

context_listbox.bind('<<ListboxSelect>>', on_select)

# Search bar section
tk.Label(scrollable_frame, text="Search for Kubernetes Object:").pack(pady=10)
search_frame = tk.Frame(scrollable_frame)
search_frame.pack(pady=10, padx=10)
search_entry = tk.Entry(search_frame, width=50)
search_entry.pack(side=tk.LEFT, padx=5)
search_button = tk.Button(search_frame, text="Search", command=search_object)
search_button.pack(side=tk.LEFT)

# Search result section
tk.Label(scrollable_frame, text="Search Results:").pack(pady=10)
search_result_text = scrolledtext.ScrolledText(scrollable_frame, width=100, height=10)
search_result_text.pack(pady=10, padx=10)

# Pod listing section
tk.Label(scrollable_frame, text="Pods:").pack(pady=10)
pod_list_text = scrolledtext.ScrolledText(scrollable_frame, width=100, height=10)
pod_list_text.pack(pady=10, padx=10)

# Service listing section
tk.Label(scrollable_frame, text="Services:").pack(pady=10)
service_list_text = scrolledtext.ScrolledText(scrollable_frame, width=100, height=10)
service_list_text.pack(pady=10, padx=10)

# Deployment listing section
tk.Label(scrollable_frame, text="Deployments:").pack(pady=10)
deployment_list_text = scrolledtext.ScrolledText(scrollable_frame, width=100, height=10)
deployment_list_text.pack(pady=10, padx=10)

# StatefulSet listing section
tk.Label(scrollable_frame, text="StatefulSets:").pack(pady=10)
statefulset_list_text = scrolledtext.ScrolledText(scrollable_frame, width=100, height=10)
statefulset_list_text.pack(pady=10, padx=10)

# DaemonSet listing section
tk.Label(scrollable_frame, text="DaemonSets:").pack(pady=10)
daemonset_list_text = scrolledtext.ScrolledText(scrollable_frame, width=100, height=10)
daemonset_list_text.pack(pady=10, padx=10)

# Ingress listing section
tk.Label(scrollable_frame, text="Ingresses:").pack(pady=10)
ingress_list_text = scrolledtext.ScrolledText(scrollable_frame, width=100, height=10)
ingress_list_text.pack(pady=10, padx=10)

app.mainloop()
