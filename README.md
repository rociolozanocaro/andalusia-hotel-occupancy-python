# Del dato crudo a la decisión: ocupación hotelera en Málaga
Es un mini-proyecto para la prueba técnica de formadora Data Analytics. El objetivo es preparar una actividad que muestre el ciclo de vida del dato desde una perspectiva técnica, pedagógica y ética.

**Pregunta principal:** ¿está la capacidad hotelera de Málaga cerca de su límite en temporada alta?

---

## Contenido del repositorio

```
├── DelDatoCrudoALaDecisión-RocíoLozanoCaro.ipynb   #https://colab.research.google.com/drive/1Z5Kn-ls1jgI842740nIgeV5bOtR_f5pH#scrollTo=36gzMiAnAWc_
├── requirements.txt                #Librerías y sus versiones
├── README.md                       #Guía didáctica
├── data/
│   ├── 2066.csv                    #Copia del CSV original
│   └── 2066_limpio.csv             #Dataset tras la limpieza
└── img/                            #
```

## Cómo ejecutarlo

1. Abre el notebook en Google Colab.
2. Ejecuta todas las celdas en orden: Ejecutar todo/Run all.
3. El notebook descarga los datos directamente desde la web del INE, así que no hay que subir ningún archivo. Si la web del INE fallara, sube `data/2066.csv` a Colab y cambia la URL por `'2066.csv'` en la celda de carga.
4. En local: `pip install -r requirements.txt`. La librería `missingno` se instala en la primera celda con `%pip install missingno`.

---