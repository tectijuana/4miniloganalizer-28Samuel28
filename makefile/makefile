# ============================================================
# Makefile — Mini Cloud Log Analyzer (Variante C)
# ============================================================
 
AS      = as
LD      = ld
TARGET  = analyzer
SRC     = analyzer.s
 
.PHONY: all clean run test
 
all: $(TARGET)
 
$(TARGET): $(SRC)
	$(AS) -o analyzer.o $(SRC)
	$(LD) -o $(TARGET) analyzer.o
 
run: $(TARGET)
	cat logs.txt | ./$(TARGET)
 
test: $(TARGET)
	@echo "=== Test 1: 503 presente ==="
	@printf "200\n404\n503\n200\n" | ./$(TARGET)
	@echo "=== Test 2: Sin 503 ==="
	@printf "200\n201\n404\n500\n" | ./$(TARGET)
	@echo "=== Test 3: 503 en primera linea ==="
	@printf "503\n200\n" | ./$(TARGET)
	@echo "=== Test 4: Multiples 503 (solo reporta el primero) ==="
	@printf "200\n503\n503\n" | ./$(TARGET)
	@echo "=== Test 5: logs.txt ==="
	cat logs.txt | ./$(TARGET)
 
clean:
	rm -f analyzer.o $(TARGET)
