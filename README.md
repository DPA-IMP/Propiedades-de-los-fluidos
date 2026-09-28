# Simulador de propiedades de los fluidos: Del pozo al tanque

Simulador didáctico de propiedades de los fluidos petroleros durante su recorrido por una instalación costa afuera. Permite explorar cómo cambian la presión, la temperatura y las propiedades PVT, con un diagrama interactivo, gráficas y una tabla por etapa.

Desarrollado en HTML, CSS y JavaScript, con gráficos SVG.

## Etapas del recorrido

| Punto | Etapa | Representación |
| --- | --- | --- |
| 1 | Yacimiento | Condiciones iniciales del fluido |
| 2 | Fondo de pozo | Inicio del ascenso y reducción de presión |
| 3 | Cabeza de pozo | Lecho marino y enfriamiento por contacto con el entorno |
| 4 | Pozo sumergido | Recorrido a través del agua hacia la instalación |
| 5 | Separador 1 | Primera etapa de separación |
| 6 | Separador 2 | Segunda etapa de separación |
| 7 | Tanque | Referencia final a condiciones estándar |

## Fluidos y condiciones iniciales

| Fluido | Presión inicial (psia) | Temperatura inicial (°F) | Referencia de saturación |
| --- | ---: | ---: | --- |
| Aceite negro | 5000 | 200 | Burbuja: 1300 psia a 200 °F |
| Aceite volátil | 5000 | 225 | Burbuja: 3500 psia a 225 °F |
| Gas y condensado | 5000 | 270 | Rocío ilustrativo: 5600 psia a 270 °F |
| Gas húmedo | 5200 | 250 | Envolvente representativa sin calibración PVT |
| Gas seco | 4800 | 240 | Envolvente representativa sin calibración PVT |

Los aceites emplean estos parámetros:

| Fluido | °API | Gravedad específica del gas, γg | Rsi (scf/STB) |
| --- | ---: | ---: | ---: |
| Aceite negro | 32 | 0.75 | 240.68 |
| Aceite volátil | 45 | 0.85 | 1306.30 |

Las gravedades específicas del gas para gas y condensado, gas húmedo y gas seco son 0.75, 0.68 y 0.60, respectivamente. Con las condiciones iniciales del gas y condensado, la presión del yacimiento ya es inferior a la referencia de rocío.

## Propiedades y unidades

| Variable | Unidad | Uso |
| --- | --- | --- |
| Presión, P | psia | Presión absoluta |
| Temperatura, T | °F | Entrada y visualización |
| Factor volumétrico del aceite, Bo | bbl/STB | Volumen de aceite a condiciones locales por volumen a condiciones estándar |
| Gas disuelto, Rs | scf/STB | Gas en solución por volumen estándar de aceite |
| Densidad | kg/m³ | De la fase aceite o gas, según el fluido seleccionado |
| Viscosidad dinámica | cP | De la fase aceite o gas |
| Presión de burbuja local, Pb | psia | Para los aceites, excepto en la referencia del tanque |
| Factor de compresibilidad, Z | Adimensional | Para los gases |
| Factor volumétrico del gas, Bg | ft³/scf | Para los gases |

Las correlaciones de gas que requieren temperatura absoluta utilizan **°R = °F + 459.67**. La densidad calculada en lb/ft³ se convierte a kg/m³; la correlación de viscosidad de Lee utiliza la densidad en g/cm³. Rs representa gas disuelto, no la relación gas–aceite total producida.

## Modelo de cálculo

### Propiedades PVT

- **Standing:** Rs, presión de burbuja y Bo saturado.
- **Bo por encima de Pb:** corrección exponencial con compresibilidad constante asumida de `0.00001 psi⁻¹`.
- **Beggs–Robinson:** viscosidad del aceite muerto y del aceite con gas disuelto, con una corrección adicional por encima de Pb.
- **Sutton:** propiedades pseudocríticas del gas a partir de su gravedad específica.
- **Beggs–Brill:** factor Z a partir de presión y temperatura pseudorreducidas.
- **Ecuación de gas real:** densidad del gas y factor Bg.
- **Lee–Gonzalez–Eakin:** viscosidad del gas.


Las condiciones del tanque son: **14.7 psia y 60 °F**, con **Rs = 0** y **Bo = 1** para el aceite estabilizado. En los casos de gas, el último punto muestra propiedades a esa referencia atmosférica; no representa almacenamiento de gas en un tanque abierto.

### Diagrama de fases

La envolvente P–T es una **referencia ilustrativa del fluido original**. El tramo de burbuja de los aceites utiliza Standing y el resto se representa mediante curvas geométricas. Los siete puntos emplean las mismas presiones y temperaturas de la tabla.

La envolvente permanece fija al cambiar de etapa y no incorpora los cambios de composición posteriores a los separadores. La posición del punto respecto de la curva no modifica los cálculos PVT ni determina fracciones de fase. La animación interpola linealmente P y T entre etapas.

**No utiliza Peng–Robinson ni un flash composicional.** La cantidad de líquido en los gases es ilustrativa. Algunas condiciones, especialmente las temperaturas bajas y el aceite volátil, implican extrapolaciones de correlaciones. El objetivo es enseñar tendencias; el modelo no sustituye datos PVT de laboratorio ni cálculos de diseño.

## Archivos y personalización

| Archivo | Contenido |
| --- | --- |
| `index.html` | Interfaz, estilos, diagrama SVG y motor de cálculo |
| `README.md` | Documentación de uso y publicación |

La paleta utiliza verde `#004A44`, guinda `#820000` y dorado `#B99056`. Los parámetros de los fluidos se encuentran en `FLUIDS`; el perfil del recorrido y los cálculos por etapa, en `makeRoute`.

## Referencias

- [Correlaciones de propiedades volumétricas — Whitson](https://wiki.whitson.com/bopvt/vol_props_correlations/)
- [Correlaciones de viscosidad — Whitson](https://wiki.whitson.com/bopvt/visc_correlations/)
- [Configurar la publicación con GitHub Pages — GitHub Docs](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
