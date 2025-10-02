# =============================================================================
# INTERPRETACIÓN DEL SUMMARY PLOT
# =============================================================================
"""
CÓMO LEER EL SUMMARY PLOT:
- Eje Y: Features ordenadas por importancia (más importante arriba)
- Eje X: Valores SHAP (contribución a la predicción)
  * Positivo (derecha): aumenta la probabilidad de la clase
  * Negativo (izquierda): disminuye la probabilidad de la clase
- Color: Valor de la feature
  * Rojo/Rosa: valores altos de la feature
  * Azul: valores bajos de la feature
- Dispersión horizontal: variabilidad del impacto de esa feature
"""

# =============================================================================
# ALTERNATIVAS DE VISUALIZACIÓN MÁS CLARAS
# =============================================================================

# 1. BAR PLOT - Importancia promedio por feature (MÁS SIMPLE)
print("=== IMPORTANCIA PROMEDIO DE FEATURES ===")
shap.plots.bar(shap_rf, max_display=10)  # Top 10 features más importantes

# 2. WATERFALL - Explicación de UNA predicción específica
print("=== EXPLICACIÓN DE UNA PREDICCIÓN INDIVIDUAL ===")
# Explicar la primera predicción del conjunto de test
shap.waterfall_plot(shap_rf[0])  # Para Random Forest
# O para una instancia específica:
instance_idx = 0  # cambiar por el índice que quieras explicar
shap.waterfall_plot(shap_rf[instance_idx])

# 3. FORCE PLOT - Explicación interactiva de una predicción
print("=== GRÁFICO DE FUERZAS PARA UNA PREDICCIÓN ===")
shap.force_plot(shap_rf.base_values[0], shap_rf.values[0], X_shap.iloc[0])

# 4. HEATMAP - Para ver patrones en múltiples predicciones
print("=== MAPA DE CALOR DE MÚLTIPLES PREDICCIONES ===")
shap.plots.heatmap(shap_rf[:100])  # Primeras 100 predicciones

# 5. BEESWARM (es el summary_plot pero con nombre más claro)
print("=== SUMMARY PLOT TRADICIONAL ===")
shap.plots.beeswarm(shap_rf, max_display=10)

# =============================================================================
# ANÁLISIS POR MODELO PARA COMPARAR
# =============================================================================

# Función para hacer análisis completo de un modelo
def analizar_modelo_shap(shap_values, X_data, model_name):
    print(f"\n{'='*50}")
    print(f"ANÁLISIS SHAP: {model_name}")
    print(f"{'='*50}")
    
    # 1. Importancia promedio
    plt.figure(figsize=(10, 6))
    shap.plots.bar(shap_values, max_display=10, show=False)
    plt.title(f'{model_name} - Top 10 Features Más Importantes')
    plt.tight_layout()
    plt.show()
    
    # 2. Summary plot
    plt.figure(figsize=(10, 8))
    shap.summary_plot(shap_values, X_data, max_display=10, show=False)
    plt.title(f'{model_name} - Distribución de Valores SHAP')
    plt.tight_layout()
    plt.show()
    
    # 3. Explicación de una predicción específica
    plt.figure(figsize=(10, 6))
    shap.waterfall_plot(shap_values[0], show=False)
    plt.title(f'{model_name} - Explicación Predicción #1')
    plt.tight_layout()
    plt.show()

# Aplicar a todos los modelos
analizar_modelo_shap(shap_rf, X_shap, "Random Forest")
analizar_modelo_shap(shap_gb, X_shap, "Gradient Boosting") 
analizar_modelo_shap(shap_rl, X_shap, "Logistic Regression")
analizar_modelo_shap(shap_mlp, X_shap, "MLP PyTorch")

# =============================================================================
# COMPARACIÓN ENTRE MODELOS
# =============================================================================

# Crear un DataFrame con la importancia promedio de cada modelo
import pandas as pd

def get_feature_importance(shap_values, feature_names):
    """Extrae la importancia promedio de features"""
    if isinstance(shap_values, list):  # Para multiclase
        # Promedio de importancia absoluta entre todas las clases
        importance = np.mean([np.abs(sv).mean(0) for sv in shap_values], axis=0)
    else:
        importance = np.abs(shap_values).mean(0)
    return pd.Series(importance, index=feature_names)

# Obtener importancias
feature_names = X_shap.columns
importance_rf = get_feature_importance(shap_rf, feature_names)
importance_gb = get_feature_importance(shap_gb.values, feature_names)
importance_rl = get_feature_importance(shap_rl.values, feature_names)
importance_mlp = get_feature_importance(shap_mlp.values, feature_names)

# Crear DataFrame comparativo
comparison_df = pd.DataFrame({
    'Random Forest': importance_rf,
    'Gradient Boosting': importance_gb,
    'Logistic Regression': importance_rl,
    'MLP PyTorch': importance_mlp
}).sort_values('Random Forest', ascending=False)

print("\n=== TOP 10 FEATURES MÁS IMPORTANTES POR MODELO ===")
print(comparison_df.head(10).round(4))

# Visualizar comparación
plt.figure(figsize=(12, 8))
comparison_df.head(10).plot(kind='bar', width=0.8)
plt.title('Comparación de Importancia de Features entre Modelos')
plt.xlabel('Features')
plt.ylabel('Importancia SHAP Promedio')
plt.legend(title='Modelos', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()