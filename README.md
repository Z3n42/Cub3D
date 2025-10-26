<div align="center">

# 🧊 Cub3D

### My first RayCaster with miniLibX

<p>
  <img src="https://img.shields.io/badge/Score-105%2F100-success?style=for-the-badge&logo=42" alt="42 Score"/>
  <img src="https://img.shields.io/badge/Team-2_Developers-blue?style=for-the-badge" alt="Team"/>
  <img src="https://img.shields.io/badge/Language-C-00599C?style=for-the-badge&logo=c&logoColor=white" alt="Language"/>
  <img src="https://img.shields.io/badge/Graphics-miniLibX-red?style=for-the-badge" alt="miniLibX"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License"/>
  <img src="https://img.shields.io/badge/42-Urduliz-000000?style=for-the-badge&logo=42&logoColor=white" alt="42 Urduliz"/>
</p>

*A "realistic" 3D graphical representation of the inside of a maze from a first-person perspective, using raycasting principles inspired by Wolfenstein 3D.*

[Installation](#%EF%B8%8F-installation) • [Usage](#-usage) • [Raycasting](#-raycasting-engine) • [Map Format](#%EF%B8%8F-map-format) • [Game](#-game-mechanics) • [Testing](#-testing)

</div>

---

## 📋 Table of Contents

- [About the Project](#-about-the-project)
- [Team](#-team)
- [Installation](#%EF%B8%8F-installation)
- [Usage](#-usage)
- [Raycasting Engine](#-raycasting-engine)
- [Map Format](#%EF%B8%8F-map-format)
- [Game Mechanics](#-game-mechanics)
- [Implementation Details](#-implementation-details)
- [Project Structure](#-project-structure)
- [Testing](#-testing)
- [What We Learned](#-what-we-learned)
- [Requirements](#-requirements)
- [License](#-license)

---

## 🎯 About the Project

**Cub3D** is a 3D maze exploration game inspired by the legendary [Wolfenstein 3D](https://www.youtube.com/watch?v=561sPCk6ByE), one of the first FPS games ever created. The project implements a **raycasting engine** to create a pseudo-3D perspective from a 2D map, rendering walls with textures and allowing real-time first-person navigation.

![Game Demo](https://github.com/iker-gonzalez/cub3d/blob/main/game.gif)

### Why Cub3D?

<table>
<tr>
<td width="50%" valign="top">

### 🎮 3D Graphics Programming
- **Raycasting algorithm** implementation
- Pseudo-3D rendering from 2D maps
- Wall texture mapping
- Perspective projection
- Real-time rendering optimization

</td>
<td width="50%" valign="top">

### 🖥️ Graphics Library (miniLibX)
- **Window management** and display
- Pixel-by-pixel rendering
- Image manipulation (XPM textures)
- Event handling (keyboard/mouse)
- Frame buffer management

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 📐 Mathematical Concepts
- **Trigonometry** (sin, cos, tan)
- Vector mathematics
- DDA algorithm for raycasting
- Distance calculations
- Field of View (FOV) projection

</td>
<td width="50%" valign="top">

### 🗺️ Map Parsing & Validation
- **File parsing** (.cub format)
- Map validation (closed walls)
- Texture loading (NO, SO, WE, EA)
- Floor/ceiling colors (RGB)
- Player position detection (N, S, E, W)

</td>
</tr>
</table>

### The Challenge

Implementing a raycasting engine requires understanding:
- How 3D perspectives work from 2D data
- Efficient rendering without a 3D graphics library
- Mathematical precision for distance and angle calculations
- Texture mapping to create realistic walls

---

## 👥 Team

This project was developed collaboratively by:

<table>
<tr>
<th>Developer</th>
<th>GitHub</th>
<th>42 Login</th>
</tr>
<tr>
<td><b>Iñigo Gonzalez</b></td>
<td><a href="https://github.com/Z3n42">@Z3n42</a></td>
<td>ingonzal</td>
</tr>
<tr>
<td><b>Iker González</b></td>
<td><a href="https://github.com/iker-gonzalez">@iker-gonzalez</a></td>
<td>ikgonzal</td>
</tr>
</table>

Working in a team required coordination on:
- Code architecture and module separation
- Raycasting algorithm optimization
- Texture management and rendering pipeline
- Map validation and error handling

---

## 🛠️ Installation

### Prerequisites

- **GCC** or **Clang** compiler
- **Make**
- **miniLibX** (included for macOS and Linux)
- **libft** (included in project)
- **get_next_line** (included in project)
- Unix-based system (Linux, macOS)

### Clone and Compile

```bash
# Clone the repository
git clone https://github.com/iker-gonzalez/cub3d.git
cd cub3d

# Compile the program
make

# Compile bonus version
make bonus

# Clean object files
make clean

# Clean everything
make fclean

# Recompile
make re
```

After running `make`, you'll have the `cub3d` executable ready to use.

---

## 🚀 Usage

### Running the Game

```bash
./cub3d [path_to_map.cub]
```

**Example:**
```bash
./cub3d maps/map.cub
```

### Controls

| Key | Action |
|-----|--------|
| **W** | Move forward |
| **S** | Move backward |
| **A** | Strafe left |
| **D** | Strafe right |
| **←** | Rotate camera left |
| **→** | Rotate camera right |
| **ESC** | Exit game |

### Map Selection

The `maps/` directory contains example maps:
- `maps/valid/` - Valid map examples
- `maps/invalid/` - Invalid maps for testing error handling
- `maps/map.cub` - Default playable map

---

## 🔧 Raycasting Engine

### What is Raycasting?

**Raycasting** is a rendering technique that creates a 3D perspective from a 2D map by casting rays from the player's position to determine what walls are visible and at what distance.

### Visual Explanation

```
Player View (3D)          Raycasting Process          2D Map
┌─────────────┐          
│             │          Player (P) in 2D map:
│    ███      │          
│   █   █     │          ╔═══╦═══╦═══╦═══╗
│  █     █    │          ║ 1 ║ 1 ║ 1 ║ 1 ║
│  █     █    │          ╠═══╬═══╬═══╬═══╣
│   █   █     │    ←──   ║ 1 ║ 0 │ 0 ║ 1 ║
│    ███      │          ╠═══╬═══╩═══╬═══╣
│             │          ║ 1 ║   P→  ║ 1 ║
└─────────────┘          ╠═══╬═══════╬═══╣
                         ║ 1 ║ 1 ║ 1 ║ 1 ║
                         ╚═══╩═══╩═══╩═══╝

For each column of pixels:
1. Cast a ray from P in direction
2. Calculate distance to wall hit
3. Draw vertical line (taller = closer)
4. Apply texture from wall orientation
```

### Algorithm Overview

**Ray Casting Process**:
1. **Initialize player**: Position (x, y) and direction angle
2. **For each screen column** (e.g., 640 columns for 640px width):
   - Calculate ray angle relative to player direction
   - Cast ray using DDA algorithm
   - Find first wall intersection
   - Calculate perpendicular distance to avoid fish-eye effect
   - Calculate wall slice height based on distance
   - Draw vertical line with appropriate texture
3. **Render floor and ceiling** with configured colors

### Key Formulas

**Perpendicular Distance** (fish-eye correction):
```
perp_dist = dist * cos(ray_angle - player_angle)
```

**Wall Height**:
```
wall_height = (SCREEN_HEIGHT / perp_dist) * WALL_HEIGHT
```

**Texture Mapping**:
```
texture_x = (int)(wall_hit_x * TEXTURE_WIDTH) % TEXTURE_WIDTH
```

---

## 🗺️ Map Format

### .cub File Structure

A `.cub` file contains texture paths, colors, and the map layout:

```
NO ./textures/north_texture.xpm
SO ./textures/south_texture.xpm
WE ./textures/west_texture.xpm
EA ./textures/east_texture.xpm

F 220,100,0
C 225,30,0

        1111111111111111111111111
        1000000000110000000000001
        1011000001110000000000001
        1001000000000000000000001
111111111011000001110000000000001
100000000011000001110111111111111
11110111111111011100000010001
11110111111111011101010010001
11000000110101011100000010001
10000000000000001100000010001
10000000000000001101010010001
11000001110101011111011110N0111
11110111 1110101 101111010001
11111111 1111111 111111111111
```

### Map Rules

| Character | Meaning |
|-----------|---------|
| `1` | Wall |
| `0` | Empty space (walkable) |
| `N` | Player start position (facing North) |
| `S` | Player start position (facing South) |
| `E` | Player start position (facing East) |
| `W` | Player start position (facing West) |
| ` ` (space) | Void (outside map boundaries) |

### Validation Requirements

✅ **Valid Map**:
- Surrounded by walls (`1`)
- Contains exactly one player spawn (`N`, `S`, `E`, `W`)
- All empty spaces are enclosed
- No holes or gaps in walls
- Valid texture paths (`.xpm` files exist)
- RGB colors in range 0-255

❌ **Invalid Map**:
- Open edges (not surrounded by walls)
- Multiple player spawns
- Missing or invalid textures
- RGB values out of range
- Invalid characters

---

## 🎮 Game Mechanics

### Player Movement

**Forward/Backward (W/S)**:
```c
new_x = player.x + cos(player.angle) * MOVE_SPEED
new_y = player.y + sin(player.angle) * MOVE_SPEED
```

**Strafe Left/Right (A/D)**:
```c
new_x = player.x + cos(player.angle + PI/2) * MOVE_SPEED
new_y = player.y + sin(player.angle + PI/2) * MOVE_SPEED
```

### Camera Rotation

**Rotate Left/Right (Arrows)**:
```c
player.angle += ROTATION_SPEED  // Right arrow
player.angle -= ROTATION_SPEED  // Left arrow
```

**Field of View (FOV)**: Typically 60-90 degrees

### Collision Detection

Before moving, check if new position hits a wall:
```c
if (map[(int)new_y][(int)new_x] == '1')
    // Block movement
else
    // Allow movement
```

---

## 💻 Implementation Details

### Data Structures

**Main Game Structure**:
```c
typedef struct s_game
{
    void        *mlx;           // MLX instance
    void        *win;           // Window pointer
    t_img       img;            // Image buffer
    t_player    player;         // Player data
    t_map       map;            // Map data
    t_textures  textures;       // Wall textures
    int         floor_color;    // Floor RGB
    int         ceiling_color;  // Ceiling RGB
}   t_game;
```

<details>
<summary><b>Player Structure</b></summary>

```c
typedef struct s_player
{
    double  x;          // X position in map
    double  y;          // Y position in map
    double  angle;      // Viewing angle (radians)
    double  fov;        // Field of view
    double  move_speed; // Movement speed
    double  rot_speed;  // Rotation speed
}   t_player;
```

</details>

<details>
<summary><b>Map Structure</b></summary>

```c
typedef struct s_map
{
    char    **grid;         // 2D array of map
    int     width;          // Map width
    int     height;         // Map height
    char    player_dir;     // Initial direction (N/S/E/W)
}   t_map;
```

</details>

<details>
<summary><b>Texture Structure</b></summary>

```c
typedef struct s_textures
{
    t_img   north;      // North wall texture
    t_img   south;      // South wall texture
    t_img   east;       // East wall texture
    t_img   west;       // West wall texture
    int     width;      // Texture width (e.g., 64)
    int     height;     // Texture height (e.g., 64)
}   t_textures;
```

</details>

<details>
<summary><b>Image Structure</b></summary>

```c
typedef struct s_img
{
    void    *img;           // Image pointer
    char    *addr;          // Pixel data address
    int     bits_per_pixel; // Bits per pixel
    int     line_length;    // Bytes per line
    int     endian;         // Endian (byte order)
}   t_img;
```

</details>

### Raycasting Implementation

<details>
<summary><b>Main Raycasting Loop (ray.c)</b></summary>

```c
void    cast_rays(t_game *game)
{
    int     x;
    double  ray_angle;
    double  angle_step;

    x = 0;
    angle_step = game->player.fov / SCREEN_WIDTH;
    ray_angle = game->player.angle - (game->player.fov / 2);

    while (x < SCREEN_WIDTH)
    {
        cast_single_ray(game, ray_angle, x);
        ray_angle += angle_step;
        x++;
    }
}
```

**Logic**: For each screen column, cast a ray at slightly different angle.

</details>

<details>
<summary><b>Single Ray Casting (ray_2.c)</b></summary>

```c
void    cast_single_ray(t_game *game, double ray_angle, int x)
{
    double  dist;
    int     wall_height;
    int     wall_side;

    // Cast ray and get distance to wall
    dist = cast_ray_dda(game, ray_angle, &wall_side);

    // Correct fish-eye effect
    dist = dist * cos(ray_angle - game->player.angle);

    // Calculate wall slice height
    wall_height = (int)(SCREEN_HEIGHT / dist);

    // Draw wall slice
    draw_wall_slice(game, x, wall_height, wall_side);
}
```

</details>

<details>
<summary><b>DDA Algorithm for Ray Casting</b></summary>

```c
double  cast_ray_dda(t_game *game, double ray_angle, int *wall_side)
{
    double  x = game->player.x;
    double  y = game->player.y;
    double  dx = cos(ray_angle);
    double  dy = sin(ray_angle);
    double  dist = 0;

    while (1)
    {
        x += dx * STEP_SIZE;
        y += dy * STEP_SIZE;
        dist += STEP_SIZE;

        // Check if hit wall
        if (game->map.grid[(int)y][(int)x] == '1')
        {
            // Determine wall orientation (N/S/E/W)
            *wall_side = determine_wall_side(dx, dy);
            return (dist);
        }

        // Safety: max distance check
        if (dist > MAX_DISTANCE)
            return (MAX_DISTANCE);
    }
}
```

**DDA (Digital Differential Analyzer)**: Step through grid incrementally until wall hit.

</details>

### Texture Mapping

<details>
<summary><b>Wall Rendering with Texture (draw.c)</b></summary>

```c
void    draw_wall_slice(t_game *game, int x, int wall_height, int side)
{
    int     y;
    int     wall_top;
    int     wall_bottom;
    t_img   *texture;
    int     tex_x;
    int     tex_y;
    int     color;

    // Select texture based on wall side
    texture = get_texture_for_side(game, side);

    // Calculate wall boundaries
    wall_top = (SCREEN_HEIGHT - wall_height) / 2;
    wall_bottom = wall_top + wall_height;

    // Calculate texture X coordinate
    tex_x = calculate_texture_x(game, side);

    // Draw ceiling
    y = 0;
    while (y < wall_top)
        put_pixel(&game->img, x, y++, game->ceiling_color);

    // Draw wall with texture
    y = wall_top;
    while (y < wall_bottom)
    {
        // Calculate texture Y coordinate
        tex_y = ((y - wall_top) * texture->height) / wall_height;

        // Get pixel color from texture
        color = get_texture_pixel(texture, tex_x, tex_y);

        // Put pixel to screen
        put_pixel(&game->img, x, y, color);
        y++;
    }

    // Draw floor
    while (y < SCREEN_HEIGHT)
        put_pixel(&game->img, x, y++, game->floor_color);
}
```

</details>

### Map Parsing

<details>
<summary><b>Parse .cub File (map_header.c)</b></summary>

```c
int parse_cub_file(t_game *game, char *filename)
{
    int     fd;
    char    *line;

    fd = open(filename, O_RDONLY);
    if (fd < 0)
        return (error_msg("Cannot open file"));

    // Parse texture paths and colors
    while (get_next_line(fd, &line) > 0)
    {
        if (ft_strncmp(line, "NO ", 3) == 0)
            game->textures.north_path = parse_texture_path(line);
        else if (ft_strncmp(line, "SO ", 3) == 0)
            game->textures.south_path = parse_texture_path(line);
        else if (ft_strncmp(line, "WE ", 3) == 0)
            game->textures.west_path = parse_texture_path(line);
        else if (ft_strncmp(line, "EA ", 3) == 0)
            game->textures.east_path = parse_texture_path(line);
        else if (ft_strncmp(line, "F ", 2) == 0)
            game->floor_color = parse_rgb_color(line);
        else if (ft_strncmp(line, "C ", 2) == 0)
            game->ceiling_color = parse_rgb_color(line);
        else if (is_map_line(line))
            break;  // Start of map grid

        free(line);
    }

    // Parse map grid
    parse_map_grid(game, fd, line);

    close(fd);
    return (0);
}
```

</details>

<details>
<summary><b>Map Validation (check_map.c)</b></summary>

```c
int validate_map(t_game *game)
{
    // Check if map is surrounded by walls
    if (!check_walls_closed(game->map))
        return (error_msg("Map not closed by walls"));

    // Check for exactly one player spawn
    if (count_player_spawns(game->map) != 1)
        return (error_msg("Invalid number of player spawns"));

    // Check for invalid characters
    if (!check_valid_characters(game->map))
        return (error_msg("Invalid characters in map"));

    // Flood fill to check for holes
    if (!flood_fill_validation(game->map))
        return (error_msg("Map has holes or open edges"));

    return (0);
}
```

</details>

### Movement and Controls

<details>
<summary><b>Key Hook Handling (ft_hook.c)</b></summary>

```c
int key_press(int keycode, t_game *game)
{
    if (keycode == KEY_ESC)
        close_game(game);
    else if (keycode == KEY_W)
        move_forward(game);
    else if (keycode == KEY_S)
        move_backward(game);
    else if (keycode == KEY_A)
        strafe_left(game);
    else if (keycode == KEY_D)
        strafe_right(game);
    else if (keycode == KEY_LEFT)
        rotate_left(game);
    else if (keycode == KEY_RIGHT)
        rotate_right(game);

    // Re-render after movement
    render_frame(game);

    return (0);
}
```

</details>

<details>
<summary><b>Movement Implementation (moves.c)</b></summary>

```c
void    move_forward(t_game *game)
{
    double  new_x;
    double  new_y;

    new_x = game->player.x + cos(game->player.angle) * game->player.move_speed;
    new_y = game->player.y + sin(game->player.angle) * game->player.move_speed;

    // Check collision
    if (game->map.grid[(int)new_y][(int)new_x] != '1')
    {
        game->player.x = new_x;
        game->player.y = new_y;
    }
}

void    strafe_left(t_game *game)
{
    double  new_x;
    double  new_y;
    double  strafe_angle;

    strafe_angle = game->player.angle - (M_PI / 2);
    new_x = game->player.x + cos(strafe_angle) * game->player.move_speed;
    new_y = game->player.y + sin(strafe_angle) * game->player.move_speed;

    if (game->map.grid[(int)new_y][(int)new_x] != '1')
    {
        game->player.x = new_x;
        game->player.y = new_y;
    }
}
```

</details>

<details>
<summary><b>Rotation Implementation (turns.c)</b></summary>

```c
void    rotate_left(t_game *game)
{
    game->player.angle -= game->player.rot_speed;

    // Normalize angle to 0-2π range
    if (game->player.angle < 0)
        game->player.angle += 2 * M_PI;
}

void    rotate_right(t_game *game)
{
    game->player.angle += game->player.rot_speed;

    // Normalize angle
    if (game->player.angle >= 2 * M_PI)
        game->player.angle -= 2 * M_PI;
}
```

</details>

---

## 📁 Project Structure

```
cub3d/
├── 📄 Makefile                  # Build configuration
├── 📄 README.md                 # Project documentation
│
├── 📂 src/                      # Mandatory source files
│   ├── main.c                   # Entry point, game loop
│   ├── cub3d.h                  # Header with structures and prototypes
│   │
│   ├── 📂 raycasting/           # Raycasting engine
│   │   ├── ray.c                # Main raycasting loop
│   │   ├── ray_2.c              # Single ray casting (DDA)
│   │   └── draw.c               # Wall/texture rendering
│   │
│   ├── 📂 textures/             # Texture parsing
│   │   ├── parse_texture_1.c    # Load textures (NO, SO, WE, EA)
│   │   ├── parse_texture_2.c    # Parse RGB colors (F, C)
│   │   └── parse_texture_3.c    # Texture utilities
│   │
│   ├── map_header.c             # Parse .cub file header
│   ├── map_utils.c              # Map utilities
│   ├── check_map.c              # Map validation (walls, spawn)
│   ├── checkutils.c             # Validation helpers
│   │
│   ├── inits.c                  # Initialize structures
│   ├── ft_hook.c                # Key event handlers
│   ├── moves.c                  # Player movement (W, S, A, D)
│   ├── turns.c                  # Camera rotation (arrows)
│   │
│   ├── mlx_functions.c          # MLX wrappers
│   ├── print_utils.c            # Debugging prints
│   ├── gets.c                   # Getters for structures
│   ├── error.c                  # Error handling
│   └── free.c                   # Memory cleanup
│
├── 📂 src_bonus/                # Bonus version (same structure)
│
├── 📂 gnl/                      # Get Next Line library
│   ├── get_next_line.c
│   ├── get_next_line.h
│   ├── get_next_line_utils.c
│   └── (bonus versions)
│
├── 📂 libft/                    # Custom C library
│   ├── ft_atoi.c, ft_split.c, ft_strlen.c, etc.
│   └── libft.h
│
├── 📂 mlx/                      # miniLibX for macOS
├── 📂 mlx_linux/                # miniLibX for Linux
│
├── 📂 maps/                     # Map files
│   ├── map.cub                  # Default map
│   ├── 📂 valid/                # Valid test maps
│   └── 📂 invalid/              # Invalid test maps
│
└── 📂 supps/                    # Valgrind suppressions
    ├── valgrind.supp
    └── valgrind_mlx.supp
```

### Module Summary

<table>
<tr>
<th>Module</th>
<th>Files</th>
<th>Purpose</th>
</tr>
<tr>
<td><b>raycasting</b></td>
<td>3</td>
<td>Core raycasting engine (ray.c, ray_2.c, draw.c)</td>
</tr>
<tr>
<td><b>textures</b></td>
<td>3</td>
<td>Texture loading and RGB parsing (parse_texture_*.c)</td>
</tr>
<tr>
<td><b>map parsing</b></td>
<td>5</td>
<td>Map file parsing and validation (map_*.c, check_*.c)</td>
</tr>
<tr>
<td><b>controls</b></td>
<td>3</td>
<td>Movement and rotation (ft_hook.c, moves.c, turns.c)</td>
</tr>
<tr>
<td><b>utils</b></td>
<td>6</td>
<td>Initialization, errors, memory, MLX wrappers</td>
</tr>
<tr>
<td><b>libraries</b></td>
<td>~50</td>
<td>gnl (6 files), libft (~40 files), mlx (included)</td>
</tr>
</table>

**Total**: ~20 core files + libraries + bonus version

<details>
<summary><b>Makefile</b></summary>

```makefile
NAME        = cub3d
NAME_BONUS  = cub3d_bonus

CC          = gcc
CFLAGS      = -Wall -Wextra -Werror -g

# Libraries
LIBFT_DIR   = libft
LIBFT       = $(LIBFT_DIR)/libft.a

GNL_DIR     = gnl
GNL_FILES   = get_next_line.c get_next_line_utils.c

# MLX (macOS / Linux)
MLX_DIR     = mlx
MLX_FLAGS   = -L$(MLX_DIR) -lmlx -framework OpenGL -framework AppKit

# Source files
SRC_DIR     = src
SRC_FILES   = main.c map_header.c map_utils.c check_map.c checkutils.c \
              inits.c ft_hook.c moves.c turns.c mlx_functions.c \
              print_utils.c gets.c error.c free.c \
              raycasting/ray.c raycasting/ray_2.c raycasting/draw.c \
              textures/parse_texture_1.c textures/parse_texture_2.c \
              textures/parse_texture_3.c

SRC         = $(addprefix $(SRC_DIR)/, $(SRC_FILES))
OBJ         = $(SRC:.c=.o)

# Bonus
BONUS_DIR   = src_bonus
BONUS_SRC   = $(subst src/,src_bonus/,$(SRC))
BONUS_OBJ   = $(BONUS_SRC:.c=.o)

# Rules
all: $(LIBFT) $(NAME)

$(NAME): $(OBJ)
	$(CC) $(CFLAGS) $^ $(LIBFT) $(MLX_FLAGS) -o $@

$(LIBFT):
	$(MAKE) -C $(LIBFT_DIR)

bonus: $(LIBFT) $(NAME_BONUS)

$(NAME_BONUS): $(BONUS_OBJ)
	$(CC) $(CFLAGS) $^ $(LIBFT) $(MLX_FLAGS) -o $@

clean:
	$(MAKE) clean -C $(LIBFT_DIR)
	$(RM) $(OBJ) $(BONUS_OBJ)

fclean: clean
	$(MAKE) fclean -C $(LIBFT_DIR)
	$(RM) $(NAME) $(NAME_BONUS)

re: fclean all

.PHONY: all clean fclean re bonus
```

</details>

---

## 🧪 Testing

### Test Categories

<table>
<tr>
<th>Test Type</th>
<th>Command</th>
<th>Expected Result</th>
</tr>
<tr>
<td><b>Valid Map</b></td>
<td><code>./cub3d maps/map.cub</code></td>
<td>Game launches successfully</td>
</tr>
<tr>
<td><b>Invalid Extension</b></td>
<td><code>./cub3d maps/map.txt</code></td>
<td>Error: Invalid file extension</td>
</tr>
<tr>
<td><b>Missing Texture</b></td>
<td><code>./cub3d maps/invalid/no_texture.cub</code></td>
<td>Error: Texture file not found</td>
</tr>
<tr>
<td><b>Open Walls</b></td>
<td><code>./cub3d maps/invalid/open_walls.cub</code></td>
<td>Error: Map not closed</td>
</tr>
<tr>
<td><b>Multiple Players</b></td>
<td><code>./cub3d maps/invalid/two_players.cub</code></td>
<td>Error: Invalid player count</td>
</tr>
<tr>
<td><b>Invalid RGB</b></td>
<td><code>./cub3d maps/invalid/bad_colors.cub</code></td>
<td>Error: RGB out of range</td>
</tr>
<tr>
<td><b>Movement Test</b></td>
<td>Press W, A, S, D in game</td>
<td>Player moves smoothly</td>
</tr>
<tr>
<td><b>Rotation Test</b></td>
<td>Press arrow keys</td>
<td>Camera rotates smoothly</td>
</tr>
</table>

### Performance Testing

```bash
# Check for memory leaks with valgrind
valgrind --leak-check=full --suppressions=supps/valgrind_mlx.supp ./cub3d maps/map.cub

# Expected: No memory leaks (except MLX internals)
```

### Map Validation Tests

**Valid Map Example** (maps/valid/simple.cub):
```
NO ./textures/north.xpm
SO ./textures/south.xpm
WE ./textures/west.xpm
EA ./textures/east.xpm

F 220,100,0
C 225,30,0

111111
100001
10N001
100001
111111
```

**Invalid Map Example** (maps/invalid/open.cub):
```
111111
10N001  ← Not closed (missing right wall)
111111
```

---

## 💡 What We Learned

Through this project, deep understanding was gained in:

- ✅ **3D Graphics Programming**: Raycasting algorithm, pseudo-3D rendering
- ✅ **Mathematical Concepts**: Trigonometry, vectors, DDA algorithm, FOV projection
- ✅ **miniLibX**: Window management, pixel rendering, image manipulation, events
- ✅ **Texture Mapping**: Applying 2D textures to 3D surfaces
- ✅ **Map Parsing**: File validation, error handling, flood fill algorithms
- ✅ **Game Development**: Real-time rendering, frame loops, input handling
- ✅ **Performance Optimization**: Efficient raycasting, minimal computations
- ✅ **Team Collaboration**: Code integration, module separation, git workflow

### Key Challenges

<table>
<tr>
<th>Challenge</th>
<th>Solution Implemented</th>
</tr>
<tr>
<td><b>Fish-eye Effect</b></td>
<td>Apply perpendicular distance correction: dist * cos(ray_angle - player_angle)</td>
</tr>
<tr>
<td><b>Texture Mapping</b></td>
<td>Calculate texture coordinates based on wall hit position and height</td>
</tr>
<tr>
<td><b>Wall Orientation</b></td>
<td>Determine side (N/S/E/W) by analyzing ray direction and hit position</td>
</tr>
<tr>
<td><b>Map Validation</b></td>
<td>Implement flood fill to check for holes and open edges</td>
</tr>
<tr>
<td><b>Smooth Movement</b></td>
<td>Use double precision for position, check collisions before moving</td>
</tr>
<tr>
<td><b>Performance</b></td>
<td>Optimize ray casting loop, minimize distance calculations</td>
</tr>
<tr>
<td><b>MLX Integration</b></td>
<td>Create custom wrappers for put_pixel, load images, handle events</td>
</tr>
<tr>
<td><b>Team Coordination</b></td>
<td>Separate modules: one handles parsing/validation, other handles rendering</td>
</tr>
</table>

### Design Decisions

**Why raycasting over full 3D engine?**
- Simpler mathematics (2D → pseudo-3D)
- Better performance for maze environments
- Educational value of understanding fundamentals
- Constraint of miniLibX (no hardware acceleration)

**Why DDA algorithm?**
- Efficient grid traversal
- Precise wall hit detection
- Avoids missing thin walls
- Standard approach in raycasting

**Why perpendicular distance correction?**
- Prevents fish-eye distortion
- Creates realistic straight walls
- Essential for proper perspective

**Why separate texture per wall side?**
- Visual variety and orientation cues
- Easier navigation (know which direction you're facing)
- Matches Wolfenstein 3D style

---

## 📏 Requirements

### Mandatory

- ✅ First-person 3D maze rendering using raycasting
- ✅ Window management (miniLibX)
- ✅ Different wall textures based on orientation (N, S, E, W)
- ✅ Different floor and ceiling colors
- ✅ Smooth player movement (W, A, S, D)
- ✅ Smooth camera rotation (arrow keys)
- ✅ Map parsing from `.cub` file
- ✅ Map validation (closed by walls, valid characters)
- ✅ Error handling for invalid maps and missing textures
- ✅ ESC key and window close button to exit
- ✅ No memory leaks

### Bonus

- ✅ Wall collisions
- ✅ Minimap system
- ✅ Doors that can open/close
- ✅ Animated sprites
- ✅ Rotate camera with mouse

### Allowed Functions

- `open`, `close`, `read`, `write`, `malloc`, `free`, `perror`, `strerror`, `exit`
- **Math library**: `-lm` (for `cos`, `sin`, `tan`, etc.)
- **miniLibX**: All functions from MLX library
- **libft**: Your own C library functions

---

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details.

This project is part of the 42 School curriculum. Feel free to use and learn from this code, but please don't copy it for your own 42 projects. Understanding comes from doing it yourself! 🚀

---

## 🔗 Related Projects

This project builds upon:

- **[libft](https://github.com/Z3n42/42_libft)** - Custom C library (used for string manipulation, memory allocation)
- **[get_next_line](https://github.com/Z3n42/get_next_line)** - Used for map file parsing
- **[FdF](https://github.com/Z3n42/fdf)** - Previous graphics project with miniLibX basics

Skills learned here apply to:
- **Game development** - Real-time rendering, game loops, input handling
- **Computer graphics** - 3D projection, texture mapping, raycasting
- **Algorithm optimization** - Performance-critical code, efficient rendering

---

<div align="center">

**Made with ☕ by [Iñigo Gonzalez](https://github.com/Z3n42) & [Iker González](https://github.com/iker-gonzalez)**

*42 Urduliz | Circle 4*

[![42 Profile - Iñigo](https://img.shields.io/badge/42_Intra-ingonzal-000000?style=flat&logo=42&logoColor=white)](https://profile.intra.42.fr/users/ingonzal)
[![42 Profile - Iker](https://img.shields.io/badge/42_Intra-ikgonzal-000000?style=flat&logo=42&logoColor=white)](https://profile.intra.42.fr/users/ikgonzal)

</div>
