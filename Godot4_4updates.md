Godot 4.4 New Nodes & Methods Summary (vs 4.3)

I. New Node Types

DetourCrowdAgent

Inherits From: Node3D

Purpose: Provides agent-based navigation using the DetourCrowd library. Allows multiple agents to navigate complex environments while avoiding each other dynamically. This is part of the new optional godot-detour module.

Key New Methods:

void set_navigation_map(RID map): Sets the navigation map the agent should use.

RID get_navigation_map() const: Gets the current navigation map RID.

void set_target_position(Vector3 position): Sets the desired destination for the agent.

Vector3 get_target_position() const: Gets the agent's target position.

Vector3 get_current_velocity() const: Gets the agent's current velocity vector.

Vector3 get_next_path_position() const: Gets the next position on the calculated path.

bool is_target_reached() const: Returns true if the agent has reached its target position.

void set_max_speed(float speed): Sets the maximum speed of the agent.

float get_max_speed() const: Gets the maximum speed.

void set_max_acceleration(float acceleration): Sets the maximum acceleration.

float get_max_acceleration() const: Gets the maximum acceleration.

void set_radius(float radius): Sets the agent's avoidance radius.

float get_radius() const: Gets the agent's radius.

void set_height(float height): Sets the agent's height.

float get_height() const: Gets the agent's height.

(Other methods exist for fine-tuning avoidance, query filters, etc.)

Key New Properties (Exposed):

navigation_map: RID

target_position: Vector3

radius: float

height: float

max_acceleration: float

max_speed: float

separation_weight: float

obstacle_avoidance_enabled: bool

(Plus others for avoidance parameters)

InstancePlaceholder

Inherits From: Node

Purpose: Primarily an editor helper. Represents an instanced scene that failed to load or is intentionally unloaded. Allows saving the scene without the missing instance data being lost. Less relevant for runtime AI logic, but technically a new node type.

Key New Methods/Properties: Primarily editor-focused, exposing stored instance path and properties. get_instance_path() might be relevant.

II. Updates to Existing Nodes (Focus on Physics / SoftBody)

SoftBody3D

Context: Received significant updates and refactoring for better usability, performance, and control in Godot 4.4 using Godot Physics (Jolt doesn't support SoftBody).

Key New Methods:

void pin_point(int point_index, bool pin): Pins or unpins a specific physics point (vertex) of the soft body. If pin is true, the point becomes kinematic (fixed or moved by animation/script, not physics). If pin is false, it becomes dynamic again. Replaces previous less direct methods.

bool is_point_pinned(int point_index) const: Checks if a specific physics point is currently pinned.

Key New Properties (Exposed): These provide more direct control over physical parameters, replacing or augmenting older simulation settings.

linear_stiffness: float: Controls the resistance to stretching or compressing along edges. (Range 0-1)

area_stiffness: float: Controls the resistance of surface triangles to deformation. (Range 0-1)

volume_stiffness: float: Controls the resistance to volume changes (if the mesh is closed). (Range 0-1)

pressure_coefficient: float: Controls the internal pressure, making the soft body inflate or deflate. Positive values inflate, negative deflate. Requires a closed mesh.

damping_coefficient: float: General damping applied to the soft body points to reduce oscillations. Replaces/clarifies older damping settings.

drag_coefficient: float: Controls air resistance affecting the soft body. Replaces/clarifies older drag settings.

Key Behavior Changes:

The simulation backend was improved. These new properties offer more intuitive control compared to the previous simulation_precision and various coefficients.

Point pinning is now more robust and directly controllable via the pin_point method.

CollisionObject2D / CollisionObject3D (Base classes for Bodies and Areas)

Context: Minor additions or refinements might occur, often related to collision layer management or signals.

Key New Methods/Properties (Check Specific Needs): While no major new methods were highlighted specifically for these base classes in 4.4 release notes, always check the latest API documentation for potential utility functions added for convenience (e.g., slight variations in signal parameters or helper functions for managing collision layers/masks). As of this summary's generation, no major new methods stand out for direct AI interaction beyond existing functionality like get_collision_layer_value, set_collision_mask_value etc.

RigidBody2D / RigidBody3D

Context: Often receive tweaks related to physics engine integration (Godot Physics, Jolt).

Key New Methods/Properties (Check Specific Needs): No major brand-new methods were heavily advertised for 4.4 runtime scripting on RigidBodies themselves. Changes often involve underlying physics server behavior, stability, or performance, not typically new script functions on the node itself. Monitor physics server changes if low-level control is needed.

CharacterBody2D / CharacterBody3D

Context: Improvements often focus on stability, edge cases (move_and_slide), or platform interaction.

Key New Methods/Properties (Check Specific Needs): No major new methods prominently featured for 4.4. Enhancements tend to be refinements to existing move_and_slide behavior (e.g., better floor detection stability, snap vector logic tweaks) rather than entirely new script functions.

PhysicsServer2D / PhysicsServer3D

Context: The low-level physics API. New functions here directly impact physics capabilities.

Key New Methods (Potential additions, verify in specific 4.4 build):

Servers often get subtle additions. Check the API docs for functions potentially related to:

New query types or parameters for *_direct_space_state.intersect_* methods.

New body states settable via body_set_state (e.g., related to sleeping or behavior flags).

Functions supporting new features in nodes (like the SoftBody3D changes might have corresponding low-level server functions, though pin_point likely wraps these).

Jolt-specific functions if using the Jolt physics engine (though these are often Jolt-specific and less portable).

Note: While specific large new universally applicable functions weren't the main highlight of 4.4 physics changes (which focused more on SoftBody node API and Jolt/GodotPhysics backend fixes/performance), always consult the exact 4.4 PhysicsServer documentation for any minor additions if needed. The SoftBody3D methods (pin_point, etc.) are the most significant user-facing physics API additions in 4.4 nodes.

III. AI Usage Notes

DetourCrowdAgent: Use this node for advanced group pathfinding and avoidance, replacing manual avoidance logic or simpler NavigationAgent use cases where dynamic inter-agent avoidance is critical. Use its methods to set destinations, control speed/acceleration, and query state.

SoftBody3D: When working with soft bodies, use the new stiffness properties (linear_stiffness, area_stiffness, volume_stiffness) for more intuitive control. Use pressure_coefficient for inflation effects. Use damping_coefficient and drag_coefficient for energy dissipation. Use pin_point and is_point_pinned for dynamically attaching/detaching parts of the soft body at runtime (e.g., anchoring a flag, picking up a soft object).

General Physics: While major new methods on rigid/character bodies weren't added, be aware that underlying physics behavior (especially with Jolt integration) might be more stable or performant in 4.4. Continue using existing methods like apply_force, apply_impulse, move_and_slide, etc.
