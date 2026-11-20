# Análisis Predictivo del Circuito ATP

Predicción de ganadores de partidos del circuito ATP (2000–2026) usando solo información disponible antes de cada partido: rating Elo, forma reciente, estadísticas de saque y devolución, historial directo y ranking.

Trabajo práctico de Ciencia de Datos, Universidad Tecnológica Nacional, Facultad Regional Córdoba (curso 5K2, 2026). Grupo N° 4.

## Resultados principales

Test: 5.582 partidos de 2024 a 2026, con corte temporal el 1 de enero de 2024.

| Métrica | Baseline (ranking) | Modelo final (ensamble) |
|---|---|---|
| Accuracy | 63,7 % | 64,6 % |
| ROC-AUC | 0,689 | 0,714 |
| Log-Loss | — | 0,6166 |
| Brier Score | — | 0,2148 |

Hallazgos:

- Sin ver el ranking, el modelo acierta 64,6 % de los partidos; el ranking solo, 63,3 %. El historial contiene la información relevante.
- El Elo es la familia de variables más influyente: al desordenarla, el ROC-AUC cae 0,066. El ranking ATP cae 0,018.
- Cuando el modelo asigna más de 90 % al favorito, este gana el 94 % de las veces (198 partidos).
- En torno al 36 % de los partidos los gana el peor rankeado: el azar pone un techo a cualquier predicción previa al partido.

No recomendamos usar el modelo para apostar: la ventaja sobre el ranking es chica y no fue validada contra cuotas reales.

## Datos

Usamos [TML-Database](https://github.com/Tennismylife/TML-Database) de Tennismylife, basada en un scraping de ATP Tour y en las bases de Jeff Sackmann.

- 78.091 partidos crudos (2000–2026) → 70.311 partidos después de la limpieza.
- 2.733 jugadores activos en el período.

El notebook descarga los CSV directamente desde GitHub. Los datos no están incluidos en este repositorio. Revisá la licencia de la fuente antes de reutilizarlos.

## Estructura del repositorio

```
atp-match-prediction/
├── README.md              Este archivo
├── LICENSE                Licencia MIT
├── requirements.txt       Versiones de librerías usadas
├── .gitignore
├── TPI_G4_Tenis.ipynb     Notebook: ETL, modelos, resultados y storytelling
├── figuras/               Gráficos del storytelling (PNG, 200 dpi)
└── paper/                 Paper en LaTeX (ES y EN) con PDF compilado
```

## Cómo reproducir el análisis

1. Usar Python 3.12 (probado con 3.12.3).
2. Instalar las dependencias:

   ```bash
   pip install -r requirements.txt
   ```

3. Abrir `TPI_G4_Tenis.ipynb` en Jupyter o Google Colab y ejecutar todas las celdas en orden. Requiere conexión a internet para descargar los datos. La búsqueda de hiperparámetros (GridSearchCV con validación temporal) tarda varios minutos.
4. Las figuras del storytelling se guardan en `figuras_storytelling/`. La carpeta `figuras/` de este repositorio contiene una copia.

## Nota de reproducibilidad

Con las versiones de `requirements.txt` se reproduce casi exactamente el notebook: la regresión logística y el Random Forest coinciden con los valores publicados, y los log-loss de los cuatro modelos también. Gradient Boosting difiere en pocas predicciones cercanas al umbral 0,5. Por eso el accuracy del ensamble puede dar 64,5 % en lugar de 64,6 %. Con otras versiones de librerías pueden variar algunas cifras en décimas.

## Limitaciones y aspectos a mejorar

El modelo no incorpora lesiones, cansancio, clima ni cuotas de mercado. Tampoco evaluamos alternativas de reentrenamiento, como ponderar los partidos recientes o limitar el histórico a partir de 2010. La evolución del rendimiento de cada jugador a lo largo de su carrera queda como trabajo futuro. El detalle está en la sección "Aspectos a mejorar" del notebook.

## Cómo citar

```bibtex
@misc{atp_match_prediction_2026,
  author       = {Angonese, Juan Pablo and Ceballos Pratto, Agustín José and Ghibaudo, Alan Yair and Scrosati, Emanuel Genaro and Villegas, Genaro and Wendler, Juan José},
  title        = {Análisis Predictivo del Circuito ATP},
  year         = {2026},
  howpublished = {\url{https://github.com/genavillegas/atp-match-prediction}},
  note         = {Trabajo práctico de Ciencia de Datos, UTN FRC}
}
```

## Licencia

Código bajo licencia MIT (ver `LICENSE`). Los datos de TML-Database tienen su propia autoría y condiciones de uso.
