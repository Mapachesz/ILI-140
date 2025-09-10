# U1: 09-09-2025  
**Funnys Company – Equipos**

---

## 1. Introducción
Funnys Company es una empresa dedicada a la producción de productos para la entretención en el hogar, con sede en Rancagua. Recientemente, la empresa ha experimento un aumento explosivo en la demanda de sus productos, lo que ha superado su capacidad de producción y red de distribución actuales.  

Para abordar este crecimiento, se evalúa la apertura de nuevas plantas de producción en cinco ciudades adicionales, las cuales son Antofagasta, Valparaíso, Santiago, Concepción y Puerto Montt, con dos opciones de capacidad pequeña o grande, y la selección entre tres alternativas de transporte (AT1, AT2, AT3) para distribuir los productos a seis regiones de Chile.  

El problema principal es observable en que la capacidad de producción y la red de distribución actualmente implementadas no permiten satisfacer la demanda creciente de las seis regiones definidas. Esto implica riesgos de desabastecimiento, mayores costos logísticos y pérdida de competitividad.  

El objetivo es determinar la configuración óptima de la red de distribución que minimice los costos totales, lo que implica responder el dónde ubicar las nuevas plantas, qué capacidad deben tener estas y su servicio de transporte, mientras se satisface la demanda proyectada para los próximos tres años.

---

## 2. Supuestos Realizados
- La demanda debe ser satisfecha completamente cada año.  
- Cada ciudad puede albergar una sola planta de producción (restricción inicial).  
- Los costos de transporte son lineales por unidad y no hay economías de escala.  
- Los costos de apertura se incurren una vez al inicio del período de planificación.  
- Los costos fijos se pagan anualmente y se consideran para los tres años.  
- La planta existente en Rancagua puede mantenerse como pequeña o upgradearse a grande, con el costo de apertura correspondiente (0 para pequeña).  
- No hay restricciones de capacidad en los servicios de transporte.  

---

## 3. Metodología de Resolución

Se empleó un modelo de programación entera mixta formulado con lo siguiente:

### Conjuntos
- Ciudades (C): Antofagasta, Valparaíso, Santiago, Concepción, Puerto Montt, Rancagua.  
- Tipos de planta (P): Pequeña, Grande.  
- Regiones (R): R1, R2, R3, R4, R5, R6.  
- Transportes (T): AT1, AT2, AT3.  
- Años (A): 1, 2, 3.  

### Parámetros
- `demanda_actual_{r}` : Demanda actual de la región r en unidades.  

- `tasas_crecimiento_{r}`: Tasa de crecimiento anual de la demanda en la región r.  

- `capacidad_planta_{p}`: Capacidad de producción de una planta de tipo p medido en unidades/año.  

- `costos_apertura_{c, p}`: Costo de apertura de una planta de tipo p en la ciudad c medido en $.  

- `costos_fijos_{c, p}`: Costo fijo anual de operación de una planta de tipo p en la ciudad c medido en $/año.  

- `costos_variables_{c, p}`: Costo variable de producción por unidad en una planta de tipo p en la ciudad c medido en $/unidad.  

- `costos_transporte_{t, c, r}`: Costo de transporte por unidad desde la ciudad c a la región r usando el transporte t medido en $/unidad.  

### Variables de Decisión
- `x_{c, p}`: Variable binaria que indica si se abre una planta de tipo p en la ciudad c.  
- `z_{c, p, a}`: Entera que representa las unidades producidas en la planta de tipo p en la ciudad c en el año a.  
- `y_{c, r, t, a}`: Entera que representa las unidades transportadas desde la ciudad c a la región r usando el transporte t en el año a.  

### Función Objetivo
Minimizar el costo total compuesto por:
- Costos de apertura de plantas.  
- Costos fijos de operación (para 3 años).  
- Costos variables de producción.  
- Costos de transporte.  


$$
\min z =
\sum_{c,p} costos\_apertura_{c,p} \cdot x_{c,p} \;+\;
3 \sum_{c,p} costos\_fijos_{c,p} \cdot x_{c,p} \;+\;
\sum_{c,p,a} costos\_variables_{c,p} \cdot z_{c,p,a} \;+\;
\sum_{c,r,t,a} costos\_transporte_{t,c,r} \cdot y_{c,r,t,a}
$$


### Restricciones
**Satisfacción de la demanda:**  

$$
\sum_{c,t} y_{c,r,t,a} \;\geq\; D_{r,a} \quad \forall r,a
$$

**Capacidad por planta:**  
$$
z_{c,p,a} \;\leq\; K_p \cdot x_{c,p} \quad \forall c,p,a
$$

**Balance producción y envíos:**  
$$
\sum_p z_{c,p,a} \;\geq\; \sum_{r,t} y_{c,r,t,a} \quad \forall c,a
$$

**A lo más una planta por ciudad (escenario base):**  
$$
\sum_p x_{c,p} \;\leq\; 1 \quad \forall c
$$

---

## 4. Configuración Óptima Recomendada

### Plantas de Producción
- **Santiago:** Implementar una planta grande con capacidad: 14,966,773 unidades/año.  
- **Rancagua:** Mantener la planta existente como pequeña con capacidad: 4,636,446 unidades/año.  
- **No se recomienda abrir plantas en otras ciudades** debido a los altos costos de apertura y operación en comparación con los beneficios.  

### Servicios de Transporte
- **Para la Región R1:** Utilizar AT2 desde Santiago.  
- **Para las Regiones R2, R3, R4, R5, R6:** Utilizar AT1 desde Santiago o Rancagua.  
- **El transporte AT3 no se utiliza** debido a sus costos más altos en la mayoría de las rutas.  

### Producción y Distribución
- La planta de Santiago cubrirá la mayor parte de la demanda, aprovechando sus bajos costos variables de producción (18.57 $/unidad) y su ubicación que reduce costos de transporte.  
- La planta de Rancagua complementará la producción, para regiones cercanas.  

**Costo Total Estimado para 3 años:** $720,505,858.72  

---

## 5. Escenario Alternativo: Múltiples Plantas por Ciudad
Si se relaja la restricción de una planta por ciudad, se recomienda abrir una planta pequeña y una grande en Santiago, debido a los bajos costos de producción y su ubicación centralizada.  

Esto permite satisfacer la demanda con menores costos de transporte, ya que se reduce la distancia a las regiones. Aunque surge otro problema: los costos de apertura y fijos aumentan. El costo total disminuye ligeramente a aproximadamente **$710 millones**, lo que representa un ahorro del 1.5% comparado al escenario base.  

Finalmente, la inversión inicial puede no justificar este ahorro, por lo que la configuración inicial sigue siendo la más recomendable.

---

## 6. Conclusiones
1. **Importancia de la localización:** La ubicación de las plantas tiene un impacto importante en los costos totales. Plantas en ciudades centrales reducen los costos de transporte debido a su proximidad a las regiones de mayor demanda.  
2. **Impacto de los costos de apertura:** Ciudades como Santiago y Rancagua tienen costos de apertura bajos (en comparación) para plantas grandes y pequeñas respectivamente, lo que las hace preferibles.  
3. **Costos de transporte:** La selección del modo de transporte adecuado puede reducir costos hasta de un 10%. AT1 es generalmente el más económico para la mayoría de las rutas, pero para regiones específicas (como R1), AT2 resulta más barato.  
4. **Costos de producción:** Los costos variables de producción son determinantes en la selección de plantas. Santiago y Rancagua tienen los costos variables más bajos, lo que las hace ideales para producción a gran escala.  
5. **Impacto de restricciones:** La restricción de una planta por ciudad limita la flexibilidad para responder a la demanda, pero el análisis muestra que relajarla no genera ahorros significativos. Por lo tanto, la configuración base es robusta y recomendada.  

**Como conclusión final**, se recomienda a Funnys Company invertir en una planta grande en Santiago y mantener la planta de Rancagua como pequeña, utilizando una combinación de AT1 y AT2 para la distribución.  

Esta configuración asegura la satisfacción de la demanda a costos mínimos y es escalable para futuros crecimientos, ante posibles nuevas restricciones y relajación de antiguas.

---

## 7. Amenazas de Validación
- **Interna:** Posibles sesgos en parámetros como tasas de crecimiento o costos de transporte; la simplificación de costos lineales podría no reflejar descuentos por volumen.  
- **Externa:** Generalización limitada si el contexto cambia (ejemplo: alza de combustibles o nuevas regulaciones); la escalabilidad a más regiones no está validada.  
- **De constructo:** Definición y operacionalización de variables depende de datos precisos; errores en estimación de demanda afectarían los resultados.  
- **De conclusión:** Si el tamaño muestral de escenarios evaluados es insuficiente o no se comparan variaciones de sensibilidad, podrían extraerse conclusiones débiles sobre robustez del modelo.  

---

## 8. Impacto y Relevancia
- **Científica:** El modelo aporta un caso aplicado de optimización de redes de distribución en Chile, integrando costos de apertura, operación y transporte en horizonte de 3 años. Transferible a otras empresas en expansión.  
- **Social:** Los consumidores de seis regiones accederán a un abastecimiento confiable de productos de entretención, mejorando la satisfacción y reduciendo riesgos de desabastecimiento. Cultural y éticamente, la propuesta no presenta barreras.  
- **Económica:** La configuración recomendada permite **minimizar costos totales** y asegurar eficiencia productiva, generando oportunidades de mercado y sostenibilidad financiera. Se evita sobreinversión en plantas poco rentables y se priorizan localizaciones estratégicas.  


## 9. Video presentacion:
https://youtu.be/JZ4UJG_-dgg
