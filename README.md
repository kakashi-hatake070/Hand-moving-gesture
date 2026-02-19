# Hand-moving-gesture
Real-time WebGL particle system using three.js and MediaPipe Hands: 12k particles with GLSL shaders, morph targets (heart, sphere, cube, spiral), gesture-controlled effects (fist/color, peace/morph, pinch/pulse, thumbs/swirl, open/glow), mirrored video preview and overlay—camera started via Start Camera.

Overview

Purpose: A browser-based real-time 3D particle system driven by hand gestures from the webcam.
File: cam.html
Main Components

Rendering: Uses three.js to render ~12,000 particles as a THREE.Points system with custom GLSL shaders (vertex-shader / fragment-shader) for soft, additive points.
Particles & Shapes: Particles have positions, colors, velocities and targetPosition attributes; precomputed morph targets: heart, sphere, cube, spiral. Morphing blends particles toward the active shape.
Shaders: Vertex shader mixes position with targetPosition using u_morphProgress; fragment shader renders round soft points with alpha falloff.
Lighting & Visuals: Hemisphere/Directional/Point lights, instanced fingertip spheres, skeletal LineSegments, and fingertip trails for a 3D hand visualization.
Input & Gesture Detection

Hands model: Uses MediaPipe Hands (@mediapipe/hands) with Camera utility to process the webcam feed in video#input-video. Start via the Start Camera button.
Smoothing: Landmarks are temporally smoothed (LANDMARK_SMOOTH_ALPHA, INDEX_SMOOTH_ALPHA) and wrist velocity is tracked for gesture dynamics.
Gestures implemented: fist, peace, pinch, thumbs (thumbs-up), open (open palm), and a fallback move. Detection helpers: detectFist, detectPeace, detectPinch, detectThumbsUp, detectOpenPalm, and gestureScore.
Behavior Mapping

Gesture → Particle effects:
fist: changes particle colors and sets cube shape.
peace: triggers morphing (heart shape).
pinch: small visual pulse; occasional burst-like effects (and a spawnBurst helper).
thumbs: toggles swirl mode and can set sphere.
open: flashes large point-size and can set spiral.
Modes: attract (default), swirl, repel with subtle visual adjustments (point size, morphing). Particle motion is heavily damped to keep effects gentle.
UI & Preview

On-screen UI: #info instructions, #gesture-ui selector for target gesture, #score display showing detected gesture and confidence.
Preview: Mirrored small video (#input-video) and overlay canvas (#overlay) that draws landmarks, connections, and per-finger correctness.
Performance & Safety

Performance choices: Heavy damping, modest per-frame updates, and instanced meshes to keep runtime reasonable despite many particles.
Graceful handling: If no hand detected the code damps velocities, hides hand visuals, and avoids large forces.
How to run

Open the page in a secure context (HTTPS or localhost), click the Start Camera button, allow camera access. Perform gestures in front of the camera to see particle/morph interactions.
