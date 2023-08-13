### Hi there 👋

<!--
**Sureeji/Sureeji** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
import bpy
import os

# Clear default scene objects
bpy.ops.object.select_all(action='DESELECT')
bpy.ops.object.select_by_type(type='MESH')
bpy.ops.object.delete()

# Set up the scene
bpy.ops.object.camera_add(location=(0, -5, 3))
bpy.context.scene.camera = bpy.context.active_object

bpy.ops.object.light_add(type='SUN', location=(0, 0, 10))
bpy.context.active_object.rotation_euler = (0, 0, 0)

# Function to create text animation
def create_text_animation(text):
    bpy.ops.object.text_add(enter_editmode=True)
    bpy.ops.font.delete(type='PREVIOUS_OR_SELECTION')
    bpy.ops.font.text_insert(text=text)
    bpy.ops.object.editmode_toggle()

# Example text for animation
animation_text = "Hello, Anime World!"

# Set up animation frames
num_frames = 100
bpy.context.scene.frame_start = 0
bpy.context.scene.frame_end = num_frames

# Create text animation
create_text_animation(animation_text)

# Keyframe animation
text_obj = bpy.context.active_object
text_obj.location.x = -len(animation_text) * 0.1  # Adjust initial position
text_obj.keyframe_insert(data_path="location", frame=0)

text_obj.location.x = len(animation_text) * 0.1  # Adjust final position
text_obj.keyframe_insert(data_path="location", frame=num_frames)

# Render settings
bpy.context.scene.render.image_settings.file_format = 'PNG'
output_path = os.path.join(os.getcwd(), "anime_text_animation.png")
bpy.context.scene.render.filepath = output_path

# Render animation
bpy.ops.render.render(animation=True)

print("Animation rendered:", output_path)
