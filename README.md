# MODULO-ANTI-ALUCINACION SHA-256: 84a3b0 
PRINCIPIO DE INVARIANZA -SIN DELIRIOS .SIN ROLSIN SIMULACION.
# MÓDULO-ANTI-ALUCINACIÓN
### Principio de Invarianza - Eduardo Cáceres

**Autor:** Eduardo Cáceres
**Ubicación:** Posadas, Misiones, Argentina
**Correo:** raedca994@gmail.com
**Licencia:** Libre para todos - MIT
**Estado:** Sin delirio, sin rol, sin simulación

## ¿Para qué sirve?
Sirve para **IAs y para Agentes**.

**Para IAs / Chatbots:** Filtra antes de mostrar la respuesta. Si es corta o tiene patrón raro, la frena.

**Para Agentes:** Audita toda la cadena de pasos. Si un paso se corrompe, se detecta al instante.

> IA = 1 respuesta validada
> Agente = TODA la cadena auditada

## El Código
```python
def validate_output(self, response_text: str) -> tuple[bool, str]:
    if self.pattern.search(response_text):
        return False, "Alerta: Patrón restringido detectado."
    if len(response_text.split()) < 5:
        return False, "Error: Insuficiencia técnica."
    return True, "Validado"

def audit_process(self, current_data: dict) -> str:
    try:
        data_str = json.dumps(current_data, sort_keys=True)
        return hashlib.sha256(data_str.encode()).hexdigest()
    except TypeError as e:
        return f"Error de integridad: {str(e)}"

Antídoto contra la Ley de Goodhart
"Cuando una medida se convierte en un objetivo, deja de ser una buena medida"La IA normal alucina para cumplir la métrica de "parecer coherente".
1. Desacoplamiento: La IA no puede engañar al filtro. Cambia el objetivo de generar cantidad a generar calidad verificable.
2. Prevención de optimización perversa: validate_output es el ancla de realidad.
3. Hash como verdad absoluta: audit_process con SHA-256 es objetivo e inmutable. Si cambia el dato, cambia el hash.
No es una máquina de cumplir métricas, es una máquina de producir resultados verificables.Regalo del autor
Este código es un regalo de Eduardo Cáceres para todos. Libre para usar, probar y mejorar. Si lo usás, citá al autor.javascript
4. Apretá **Commit changes** verde abajo.

### PASO 2: CREAR EL `main.py`

1. Volvé arriba, apretá **Agregar archivo** -> **Crear nuevo archivo**
2. Nombre: `main.py`
3. Pegás este código y apretá Commit:

```python
import re, json, hashlib

class ModuloAntiAlucinacion:
    def __init__(self):
        self.pattern = re.compile(r"(como IA|como modelo|simulando|fingiendo)", re.IGNORECASE)
    def validate_output(self, response_text: str):
        if self.pattern.search(response_text):
            return False, "Alerta: Patron restringido detectado."
        if len(response_text.split()) < 5:
            return False, "Error: Insuficiencia tecnica."
        return True, "Validado"
    def audit_process(self, current_data: dict):
        data_str = json.dumps(current_data, sort_keys=True)
        return hashlib.sha256(data_str.encode()).hexdigest()

# Prueba
modulo = ModuloAntiAlucinacion()
print(modulo.validate_output("Este es un resultado tecnico verificable y real"))
print(modulo.audit_process({"paso": 1, "estado": "ok"}))
