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

"""Módulo Anti-Alucinación y Auditoría de Integridad.

Autor original: Eduardo Cáceres.
"""

from collections.abc import Mapping
import hashlib
import json
import re
from typing import Any


class IntegrityAuditError(ValueError):
  """Excepción específica para fallos de serialización o integridad."""


class ModuloAntiAlucinacion:
  MIN_WORDS: int = 5

  def __init__(self) -> None:
    self.pattern: re.Pattern[str] = re.compile(
        r"\b("
        r"como (?:una? )?(?:IA|inteligencia artificial|modelo(?: de lenguaje)?)"
        r"|simulando|fingiendo"
        r")\b",
        re.IGNORECASE,
    )

  def validate_output(self, response_text: str) -> tuple[bool, str]:
    if not isinstance(response_text, str) or not response_text.strip():
      return False, "Error: Respuesta vacía o inválida."

    if self.pattern.search(response_text):
      return False, "Alerta: Patrón restringido detectado."

    if len(response_text.split(None, self.MIN_WORDS)) < self.MIN_WORDS:
      return False, "Error: Insuficiencia técnica."

    return True, "Validado"

  @staticmethod
  def _canonicalize(obj: Any) -> Any:
    """Normaliza tipos no nativos JSON de forma determinista."""
    if isinstance(obj, set):
      return sorted(
          (ModuloAntiAlucinacion._canonicalize(x) for x in obj), key=str
      )
    if isinstance(obj, Mapping):
      return {
          str(k): ModuloAntiAlucinacion._canonicalize(v)
          for k, v in obj.items()
      }
    if isinstance(obj, (list, tuple)):
      return [ModuloAntiAlucinacion._canonicalize(x) for x in obj]
    return obj

  def audit_process(self, current_data: Mapping[str, Any]) -> str:
    if not isinstance(current_data, Mapping):
      raise IntegrityAuditError(
          "Se esperaba un Mapping/dict, se recibió"
          f" {type(current_data).__name__}."
      )

    try:
      normalized = self._canonicalize(current_data)
      data_str = json.dumps(
          normalized,
          sort_keys=True,
          ensure_ascii=False,
          separators=(",", ":"),
          default=str,
      )
      return hashlib.sha256(data_str.encode("utf-8")).hexdigest()
    except Exception as exc:
      raise IntegrityAuditError(
          f"Fallo al serializar los datos de auditoría: {exc}"
      ) from exc


if __name__ == "__main__":
  modulo = ModuloAntiAlucinacion()
  print(
      modulo.validate_output("Este es un resultado técnico verificable y real")
  )
  print(modulo.audit_process({"paso": 1, "estado": "ok", "tags": {"b", "a"}}))
 
