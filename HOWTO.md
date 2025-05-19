# 🧩 Creating New Lessons - Developer Guide

Welcome to the Pygame-Tutor Educational Platform!  
This guide will help you create engaging, interactive lessons  
that fit seamlessly into the modern package structure.

## 📋 Lesson Structure Basics

Every lesson should:
1. Inherit from `BaseLesson`
2. Be named `lesson_XX_name.py` (XX = lesson number)
3. Be placed in the `pygametutor/lessons/` directory
4. Contain a single class matching the filename (PascalCase)

**Template:**
```python
#!/usr/bin/env python
from pygametutor.core.base import BaseLesson
from pygametutor.settings.constants import COLORS

class LessonXXName(BaseLesson):
    """Brief docstring explaining the lesson"""

    def __init__(self, screen_geo):
        super().__init__(screen_geo)
        self.title = "My Lesson Name"

        # Initialize your lesson state here
        self.example_property = 42

    def update(self, dt: float) -> bool:
        """Update game state
        Args:
            dt: Delta time since last frame (in seconds)
        Returns:
            bool: True if lesson should advance to next
        """
        # Your update logic here
        return False

    def draw(self) -> None:
        """Render all graphics"""
        super().draw()  # Draws background and title

        if not self.code_viewer.show_code:
            # Your drawing code here
            pass

    def handle_events(self, event) -> bool:
        """Process input events
        Returns:
            bool: True if event was handled
        """
        handled = super().handle_events(event)
        if not handled:
            # Your event handling here
            pass
        return handled
```

## 🗂️ New Package Structure

```
pygametutor/
├── lessons/           # All lessons go here
│   ├── lesson_XX_name.py
│   ├── img/           # Lesson-specific assets
├── core/              # Framework core
├── settings/          # Configuration
main.py                # Entry point
```

## 🛠️ Essential Components

### 1. Import Paths
Use absolute imports from the `pygametutor` package:
```python
from pygametutor.core.base import BaseLesson
from pygametutor.settings.constants import COLORS
from pygametutor.lessons import lesson_utils  # If you create shared utilities
```

### 2. Asset Loading
For lesson-specific assets:
```python
from pathlib import Path
import pygame

# Load images from the lessons/img directory
image_path = Path(__file__).parent / "img" / "character.png"
try:
    self.image = pygame.image.load(str(image_path))
except:
    # Fallback if image missing
    self.image = pygame.Surface((50, 50))
    self.image.fill(COLORS["highlight"])
```

### 3. Screen Geometry (unchanged)
```python
self.center_x  # Screen center X
self.center_y  # Screen center Y
self.width     # Screen width
self.height    # Screen height
```

## 🎨 Updated Drawing Examples

### Using the New Structure
```python
def draw(self):
    super().draw()

    if not self.code_viewer.show_code:
        # Draw lesson-specific elements
        pygame.draw.circle(
            self.screen,
            COLORS["accent"],
            (self.center_x, self.center_y),
            30
        )
```

## 🕹️ Input Handling (unchanged but with proper imports)

```python
def handle_events(self, event):
    handled = super().handle_events(event)

    if event.type == pygame.KEYDOWN:
        if event.key == pygame.K_SPACE:
            # Handle space press
            handled = True

    return handled
```

## 📚 Educational Elements

### Adding Concept Panels
```python
def _draw_concept_panel(self):
    concepts = [
        "PHYSICS CONCEPTS:",
        "• Newton's First Law",
        "• Elastic Collisions"
    ]

    y = 100
    for line in concepts:
        text = self.font.render(line, True, COLORS["text"])
        self.screen.blit(text, (50, y))
        y += 30
```

## 🧪 Testing Your Lesson

1. **Install in development mode**:
```bash
uv pip install -e .
```

2. **Run your lesson**:
```bash
pygame-tutor
```

3. **Debugging Tips**:
```python
# Print debug info (visible in terminal)
print(f"Object position: {self.obj_pos}")

# Visual debug overlay
pygame.draw.rect(self.screen, (255,0,0,100), debug_rect, 1)
```

## 📦 Example Lesson: Bouncing Ball

```python
from pygametutor.core.base import BaseLesson
from pygametutor.settings.constants import COLORS

class LessonXXBounce(BaseLesson):
    """Demonstrates basic physics with a bouncing ball"""

    def __init__(self, screen_geo):
        super().__init__(screen_geo)
        self.title = "Bouncing Ball"
        self.ball_pos = [self.center_x, self.center_y]
        self.ball_vel = [2.3, -1.5]
        self.gravity = 0.2

    def update(self, dt):
        # Physics update
        self.ball_vel[1] += self.gravity
        self.ball_pos[0] += self.ball_vel[0]
        self.ball_pos[1] += self.ball_vel[1]

        # Wall collisions
        if self.ball_pos[0] <= 20 or self.ball_pos[0] >= self.width-20:
            self.ball_vel[0] *= -1
        if self.ball_pos[1] >= self.height-20:
            self.ball_vel[1] *= -0.9
            self.ball_pos[1] = self.height-20

    def draw(self):
        super().draw()
        pygame.draw.circle(
            self.screen,
            COLORS["accent"],
            (int(self.ball_pos[0]), int(self.ball_pos[1])),
            20
        )
```

## 🚀 Publishing Your Lesson

1. Place your file in `pygametutor/lessons/`
2. Follow naming convention: `lesson_XX_name.py`
3. Add any assets to `pygametutor/lessons/img/`
4. The loader will automatically detect it

> Pro Tip: Study existing lessons like `lesson_04_breakout.py` for advanced patterns!

Happy coding! 🎉
