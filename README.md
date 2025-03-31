import pygame import random

pygame.init()

Konstanta

WIDTH, HEIGHT = 400, 400 GRID_SIZE = 4 TILE_SIZE = WIDTH // GRID_SIZE FONT = pygame.font.Font(None, 50)

Warna

WHITE = (255, 255, 255) BLACK = (0, 0, 0)

Inisialisasi layar

screen = pygame.display.set_mode((WIDTH, HEIGHT)) pygame.display.set_caption("Sliding Puzzle")

Membuat puzzle yang dapat diselesaikan

def is_solvable(puzzle): inv_count = 0 for i in range(len(puzzle)): for j in range(i + 1, len(puzzle)): if puzzle[i] and puzzle[j] and puzzle[i] > puzzle[j]: inv_count += 1 return inv_count % 2 == 0

def generate_puzzle(): numbers = list(range(1, GRID_SIZE**2)) + [0] while True: random.shuffle(numbers) if is_solvable(numbers): return numbers

numbers = generate_puzzle()

def draw_grid(): screen.fill(WHITE) for i in range(GRID_SIZE): for j in range(GRID_SIZE): num = numbers[i * GRID_SIZE + j] if num != 0: pygame.draw.rect(screen, BLACK, (j * TILE_SIZE, i * TILE_SIZE, TILE_SIZE, TILE_SIZE)) text = FONT.render(str(num), True, WHITE) screen.blit(text, (j * TILE_SIZE + TILE_SIZE//3, i * TILE_SIZE + TILE_SIZE//3)) pygame.display.update()

def move_tile(pos): x, y = pos[0] // TILE_SIZE, pos[1] // TILE_SIZE zero_idx = numbers.index(0) zero_x, zero_y = zero_idx % GRID_SIZE, zero_idx // GRID_SIZE

if (abs(x - zero_x) == 1 and y == zero_y) or (abs(y - zero_y) == 1 and x == zero_x):
    numbers[zero_idx], numbers[y * GRID_SIZE + x] = numbers[y * GRID_SIZE + x], numbers[zero_idx]

if numbers == list(range(1, GRID_SIZE**2)) + [0]:
    print("Selamat! Anda menyelesaikan puzzle!")

def main(): running = True while running: draw_grid() for event in pygame.event.get(): if event.type == pygame.QUIT: running = False if event.type == pygame.MOUSEBUTTONDOWN: move_tile(event.pos) pygame.quit()

if name == "main": main()

