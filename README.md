# Limpieza de datos — Cuentas ILMS Koha

Auditoría de calidad sobre el reporte de cuentas del Sistema Integrado de Gestión de Bibliotecas (ILMS) Koha de la Universidad de Antioquia (~95 mil registros).

**Propósito:** diagnóstico, no mutación. Genera CSVs accionables por cada tipo de anomalía para que el equipo DBA los corrija directamente en Koha.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/caamilo03/limpieza-cuentas-koha/blob/main/notebook.ipynb)

---

## Anomalías detectadas

| # | Anomalía | Descripción |
|---|---|---|
| 01 | Sin cédula | `cardnumber` nulo o vacío |
| 02 | Cédulas repetidas | Mismo `cardnumber` en varias cuentas |
| 03 | Cédula con X | `cardnumber` empieza o termina con `X` |
| 04 | Correos repetidos | Mismo `email` en varias cuentas |
| 05 | Cuentas genéricas | Heurística: surname==firstname, nombres tipo "admin/test/prueba", cédulas alfanuméricas en categorías no-STAFF |
| 06 | Préstamo interbibliotecario | `categorycode == PREIN` |
| 07 | Campos nulos | `surname`, `firstname`, `email` o `userid` vacíos |
| 08 | Empleados CIS | ⏸ Pendiente de criterio |
| X1 | Correo formato inválido | No cumple patrón básico `usuario@dominio.tld` |
| X2 | Correo no institucional | Correo fuera de `@udea.edu.co` en categorías que lo requieren |
| X3 | userid duplicado | Mismo `userid` en varias cuentas |
| X4 | Cédula longitud atípica | Cédula numérica con < 6 o > 10 dígitos |
| X5 | Espacios extremos | Espacios al inicio o final en campos clave |
| X6 | categorycode inválido | Código de categoría no existe en la tabla de categorías |

---

## Datos de entrada

Los CSV originales contienen datos personales (cédulas, nombres, correos) y **no se incluyen en este repositorio**.

Están alojados en una carpeta de Google Drive restringida a la comunidad UdeA:  
📁 [limpieza-cuentas-koha en Drive](https://drive.google.com/drive/folders/1CvzJBGmVTG68cfW9dP4l_niE_SXJEm-g?usp=sharing)

Para ejecutar el notebook necesitas una cuenta `@udea.edu.co` con acceso a esa carpeta.

---

## Cómo ejecutar

1. Haz clic en el badge **Open in Colab** de arriba.
2. En la primera celda de código (`MODO`), verifica que el valor sea `'drive'`.
3. Ajusta `RUTA_DRIVE` si tu Drive tiene los archivos en una ruta distinta.
4. `Runtime → Run all`.
5. Se te pedirá autorizar el acceso a tu Drive — acepta con tu cuenta `@udea.edu.co`.
6. Los archivos de resultado quedan en la carpeta `output/` dentro de Colab y se pueden descargar desde el panel lateral de archivos.

---

## Estructura del repositorio

```
limpieza-cuentas-koha/
├── notebook.ipynb               # Notebook principal
├── data/
│   └── ejemplo_anonimizado.csv  # 20 filas fake para smoke tests sin Drive
├── output/                      # Generado por el notebook (no versionado)
├── requirements.txt
└── .gitignore
```

---

## Ejecutar localmente (sin Colab)

```bash
pip install -r requirements.txt
jupyter notebook notebook.ipynb
```

Cambia `MODO = 'local'` en la primera celda y coloca los CSV en `data/`.
