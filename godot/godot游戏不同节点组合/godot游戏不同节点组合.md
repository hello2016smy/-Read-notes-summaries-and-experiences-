# godot游戏不同节点组合

## 太空射手

### gd

- ParallaxBackground (差背景,平行地面,平行地面)
  - ParallaxLayer (视差图层,平行层)
    - Sprite2D

---

### main

- node2D
  - --> gd场景
  - --> player场景
  - asteroid_timer

---

### player

- Area2D
  - AnimatedSprite2D
  - CollisionShape2D

---

### asteroid

- Area2D
  - AnimatedSprite2D
  - CollisionShape2D

---

### explosion

- Sprite2D
  - explosion_timer

---

### shot

- Area2D
  - AnimatedSprite2D
  - CollisionShape2D