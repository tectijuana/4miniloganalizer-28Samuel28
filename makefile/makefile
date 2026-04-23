# ============================================================
# Makefile — Mini Cloud Log Analyzer (Variante C)
# Detectar el primer evento crítico 503
# ============================================================
 
AS      = as
LD      = ld
TARGET  = analyzer
SRC     = analyzer.s
 
.PHONY: all clean run test test-unit test-mockaroo
 
# ── Compilación ─────────────────────────────────────────────
all: $(TARGET)
 
$(TARGET): $(SRC)
	@echo "[1/2] Ensamblando $(SRC)..."
	$(AS) -o analyzer.o $(SRC)
	@echo "[2/2] Enlazando..."
	$(LD) -o $(TARGET) analyzer.o
	@echo "Compilacion exitosa -> ./$(TARGET)"
 
# ── Ejecución con logs.txt (generado con Mockaroo) ──────────
run: $(TARGET)
	@echo "=== Ejecutando con logs.txt (datos Mockaroo) ==="
	cat logs.txt | ./$(TARGET)
 
# ── Todos los tests ─────────────────────────────────────────
test: test-unit test-mockaroo
 
# ── Tests unitarios (casos controlados) ─────────────────────
test-unit: $(TARGET)
	@echo ""
	@echo "=== TESTS UNITARIOS (Variante C) ==="
	@echo ""
	@echo "[ TEST 1 ] 503 en linea 3 (debe reportar linea 3)"
	@printf "200\n404\n503\n200\n" | ./$(TARGET)
	@echo "[ TEST 2 ] Sin ningun 503 (debe reportar no encontrado)"
	@printf "200\n201\n404\n500\n" | ./$(TARGET)
	@echo "[ TEST 3 ] 503 en primera linea (debe reportar linea 1)"
	@printf "503\n200\n404\n" | ./$(TARGET)
	@echo "[ TEST 4 ] Multiples 503 - solo reporta el primero (linea 2)"
	@printf "200\n503\n404\n503\n" | ./$(TARGET)
	@echo "[ TEST 5 ] Codigos 5xx variados, 503 en linea 4"
	@printf "500\n502\n504\n503\n" | ./$(TARGET)
	@echo "[ TEST 6 ] Solo 200s - no debe encontrar 503"
	@printf "200\n200\n200\n200\n200\n" | ./$(TARGET)
	@echo "[ TEST 7 ] Entrada vacia - no debe encontrar 503"
	@printf "" | ./$(TARGET)
	@echo "[ TEST 8 ] Un solo codigo que ES 503"
	@printf "503\n" | ./$(TARGET)
	@echo ""
	@echo "Tests unitarios completados"
 
# ── Test con datos reales de Mockaroo ────────────────────────
test-mockaroo: $(TARGET) logs.txt
	@echo ""
	@echo "=== TEST CON DATOS MOCKAROO (100 filas) ==="
	@echo ""
	@echo "Primeras 10 lineas de logs.txt:"
	@head -10 logs.txt
	@echo "..."
	@echo ""
	@echo "Resultado del analisis:"
	@cat logs.txt | ./$(TARGET)
	@echo ""
	@echo "Estadisticas del archivo:"
	@echo "  Total de registros : $$(wc -l < logs.txt)"
	@echo "  Cantidad de 503s   : $$(grep -c '^503$$' logs.txt || echo 0)"
	@echo "  Primera linea con 503: $$(grep -n '^503$$' logs.txt | head -1 | cut -d: -f1 || echo ninguna)"
	@echo ""
	@echo "Test Mockaroo completado"
 
# ── Limpieza ─────────────────────────────────────────────────
clean:
	@rm -f analyzer.o $(TARGET)
	@echo "Archivos objeto eliminados"
