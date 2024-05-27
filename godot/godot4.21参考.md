# godot4.21参考



## 类：   Node

继承：   Object
派生：   AnimationMixer ,   AudioStreamPlayer ,   CanvasItem ,   CanvasLayer ,   EditorFileSystem ,   EditorPlugin ,   EditorResourcePreview ,   HTTPRequest ,   InstancePlaceholder ,   MissingNode ,   MultiplayerSpawner ,   MultiplayerSynchronizer ,   NavigationAgent2D ,   NavigationAgent3D ,   Node3D ,   ResourcePreloader ,   ShaderGlobalsOverride ,   SkeletonIK3D ,   Timer ,   Viewport ,   WorldEnvironment


所有场景对象的基类。


描述

节点是 Godot 的构建模块。它们可以被指定为另一个节点的子节点，从而形成树状排列。一个给定的节点可以包含任意数量的节点作为子节点，要求所有的兄弟节点（即该节点的直接子节点）的名字唯一。

节点树被称为场景。场景可以被保存到磁盘上，然后被实例化到其他场景中。这使得 Godot 项目的架构和数据模型具有非常高的灵活性。

场景树：SceneTree 包含活动的节点树。当一个节点被添加到场景树中时，它将收到 NOTIFICATION_ENTER_TREE 通知，并触发其 _enter_tree() 回调。子节点总是在其父节点之后被添加，即父节点的 _enter_tree() 回调将在其子节点的之前被触发。

一旦所有的节点被添加到场景树中，它们就会收到 NOTIFICATION_READY 通知，其各自的 _ready() 回调被触发。对于一组节点，_ready() 回调是按相反的顺序调用的，从子节点开始，向上移动到父节点。

这意味着，当把一个节点添加到场景树中时，将使用下面的顺序进行回调：父节点的 _enter_tree()、子节点的 _enter_tree()、子节点的 _ready()，最后是父节点的 _ready()（对整个场景树进行递归）。

处理：节点可以覆盖“处理”状态，以便它们在每一帧上都收到回调，要求它们进行处理（做一些事情）。普通处理（回调 _process()，可以使用 set_process() 开关）会尽可能快地发生，并且取决于帧率，所以处理时间 delta（单位为秒）会作为参数传入。物理处理（回调 _physics_process()，可以使用 set_physics_process() 开关）每秒发生固定次数（默认为 60），对物理引擎相关的代码很有用。

节点也可以处理输入事件。存在 _input() 函数时，程序每收到一次输入都会去调用它。在许多情况下，这么做是大材小用了（除非是用于简单的项目），用 _unhandled_input() 函数可能更合适；当输入事件没有被其他节点（通常是 GUI Control 节点）处理时，才会调用这个函数，可以确保节点只接收到它该收到的事件。

为了记录场景的层次结构（尤其是在将场景实例化到其他场景时）可以用 owner 属性为节点设置一个“所有者”。它记录的是谁实例化了什么。这在编写编辑器和工具时非常有用。

最后，当一个节点被 Object.free() 或 queue_free() 释放时，它也将释放它的所有子节点。

分组：节点可以被添加到很多的组中，以方便管理，你可以根据自己游戏的需要来创建类似“敌人”或“收集品”这样的组。见 add_to_group()、is_in_group() 和 remove_from_group()。加入组后，你可以检索这些组中的所有节点，对它们进行迭代，甚至通过 SceneTree 中的方法调用组内方法。

节点的网络编程：在连接到服务器（或制作服务器，见 ENetMultiplayerPeer）之后，可以使用内置的 RPC（远程过程调用）系统在网络上进行通信。在调用 rpc() 时传入方法名，将在本地和所有已连接的对等体中调用对应的方法（对等体=客户端和接受连接的服务器）。为了识别哪个节点收到 RPC 调用，Godot 将使用它的 NodePath（请确保所有对等体上的节点名称相同）。另外，请参阅高级网络教程和相应的演示。

注意：script 属性是 Object 类的一部分，不属于 Node。这个属性暴露的方式和其他属性不同，但提供了 setter 和 getter（set_script() 和 get_script()）。


在线教程
节点与场景
所有演示


属性
Stringeditor_description [默认： ""]MultiplayerAPImultiplayerStringNamenameNodeownerProcessModeprocess_mode [默认： 0]intprocess_physics_priority [默认： 0]intprocess_priority [默认： 0]ProcessThreadGroupprocess_thread_group [默认： 0]intprocess_thread_group_orderBitField[ProcessThreadMessages]process_thread_messagesStringscene_file_pathboolunique_name_in_owner [默认： false]

方法
void_enter_tree() virtualvoid_exit_tree() virtualPackedStringArray_get_configuration_warnings() virtual constvoid_input(event: InputEvent) virtualvoid_physics_process(delta: float) virtualvoid_process(delta: float) virtualvoid_ready() virtualvoid_shortcut_input(event: InputEvent) virtualvoid_unhandled_input(event: InputEvent) virtualvoid_unhandled_key_input(event: InputEvent) virtualvoidadd_child(node: Node, force_readable_name: bool = false, internal: InternalMode = 0)voidadd_sibling(sibling: Node, force_readable_name: bool = false)voidadd_to_group(group: StringName, persistent: bool = false)Variantcall_deferred_thread_group(method: StringName, ...) varargVariantcall_thread_safe(method: StringName, ...) varargboolcan_process() constTweencreate_tween()Nodeduplicate(flags: int = 15) constNodefind_child(pattern: String, recursive: bool = true, owned: bool = true) constArray[Node]find_children(pattern: String, type: String = "", recursive: bool = true, owned: bool = true) constNodefind_parent(pattern: String) constNodeget_child(idx: int, include_internal: bool = false) constintget_child_count(include_internal: bool = false) constArray[Node]get_children(include_internal: bool = false) constArray[StringName]get_groups() constintget_index(include_internal: bool = false) constWindowget_last_exclusive_window() constintget_multiplayer_authority() constNodeget_node(path: NodePath) constArrayget_node_and_resource(path: NodePath)Nodeget_node_or_null(path: NodePath) constNodeget_parent() constNodePathget_path() constNodePathget_path_to(node: Node, use_unique_path: bool = false) constfloatget_physics_process_delta_time() constfloatget_process_delta_time() constboolget_scene_instance_load_placeholder() constSceneTreeget_tree() constStringget_tree_string()Stringget_tree_string_pretty()Viewportget_viewport() constWindowget_window() constboolhas_node(path: NodePath) constboolhas_node_and_resource(path: NodePath) constboolis_ancestor_of(node: Node) constboolis_displayed_folded() constboolis_editable_instance(node: Node) constboolis_greater_than(node: Node) constboolis_in_group(group: StringName) constboolis_inside_tree() constboolis_multiplayer_authority() constboolis_node_ready() constboolis_physics_processing() constboolis_physics_processing_internal() constboolis_processing() constboolis_processing_input() constboolis_processing_internal() constboolis_processing_shortcut_input() constboolis_processing_unhandled_input() constboolis_processing_unhandled_key_input() constvoidmove_child(child_node: Node, to_index: int)voidnotify_deferred_thread_group(what: int)voidnotify_thread_safe(what: int)voidprint_orphan_nodes() staticvoidprint_tree()voidprint_tree_pretty()voidpropagate_call(method: StringName, args: Array = [], parent_first: bool = false)voidpropagate_notification(what: int)voidqueue_free()voidremove_child(node: Node)voidremove_from_group(group: StringName)voidreparent(new_parent: Node, keep_global_transform: bool = true)voidreplace_by(node: Node, keep_groups: bool = false)voidrequest_ready()Errorrpc(method: StringName, ...) varargvoidrpc_config(method: StringName, config: Variant)Errorrpc_id(peer_id: int, method: StringName, ...) varargvoidset_deferred_thread_group(property: StringName, value: Variant)voidset_display_folded(fold: bool)voidset_editable_instance(node: Node, is_editable: bool)voidset_multiplayer_authority(id: int, recursive: bool = true)voidset_physics_process(enable: bool)voidset_physics_process_internal(enable: bool)voidset_process(enable: bool)voidset_process_input(enable: bool)voidset_process_internal(enable: bool)voidset_process_shortcut_input(enable: bool)voidset_process_unhandled_input(enable: bool)voidset_process_unhandled_key_input(enable: bool)voidset_scene_instance_load_placeholder(load_placeholder: bool)voidset_thread_safe(property: StringName, value: Variant)voidupdate_configuration_warnings()

信号

● child_entered_tree(node: Node)在子节点进入场景树时触发，可以是因为该子节点自行进入，也可以是因为本节点带着该子节点一起进入。

这个信号会在该子节点自身的 NOTIFICATION_ENTER_TREE 和 tree_entered 之后触发。

● child_exiting_tree(node: Node)当一个子节点即将退出场景树时发出，要么是因为它正在被移除或直接释放，要么是因为该节点正在退出树。

当收到这个信号时，子 node 仍然在树中并且有效。该信号在子节点自己的 tree_exiting 和 NOTIFICATION_EXIT_TREE 之后发出。

● child_order_changed()子节点列表发生改变时发出。发生在添加、移动、移除子节点时。

● ready()当该节点就绪时发出。在 _ready() 回调之后发出，遵循相同的规则。

● renamed()当该节点被重命名时触发。

● replacing_by(node: Node)当该节点被 node 替换时触发，见 replace_by()。

这个信号的触发时机在 node 被添加为原父节点的子节点之后，但是在所有原子节点重设父节点为 node 之前。

● tree_entered()当该节点进入树时触发。

这个信号会在相关的 NOTIFICATION_ENTER_TREE 通知之后触发。

● tree_exited()当该节点退出树之后触发，并且不再处于活动状态。

● tree_exiting()当该节点仍处于活动状态但即将退出树时发出。这是反初始化的正确位置（如果愿意，也可以称之为“析构函数”）。

这个信号会在相关的 NOTIFICATION_EXIT_TREE 通知之前触发。


枚举
enum  ProcessMode:

● PROCESS_MODE_INHERIT = 0
从该节点的父节点继承处理模式。如果是根节点，则等价于 PROCESS_MODE_PAUSABLE。默认值。
● PROCESS_MODE_PAUSABLE = 1
SceneTree 暂停时停止处理（取消暂停时处理）。与 PROCESS_MODE_WHEN_PAUSED 相反。
● PROCESS_MODE_WHEN_PAUSED = 2
仅在 SceneTree 暂停时处理（取消暂停时不处理）。与 PROCESS_MODE_PAUSABLE 相反。
● PROCESS_MODE_ALWAYS = 3
始终处理。始终继续处理，忽略 SceneTree 的 paused 属性。与 PROCESS_MODE_DISABLED 相反。
● PROCESS_MODE_DISABLED = 4
从不处理。完全禁用处理，忽略 SceneTree 的 paused 属性。与 PROCESS_MODE_ALWAYS 相反。

enum  ProcessThreadGroup:

● PROCESS_THREAD_GROUP_INHERIT = 0
如果 process_thread_group 属性设为这个值，该节点会属于线程组不是继承的父节点（或祖父节点）。详见 process_thread_group。
● PROCESS_THREAD_GROUP_MAIN_THREAD = 1
在主线程上处理该节点（以及设为继承的子节点）。详见 process_thread_group。
● PROCESS_THREAD_GROUP_SUB_THREAD = 2
在子线程上处理该节点（以及设为继承的子节点）。详见 process_thread_group。

flags  ProcessThreadMessages:

● FLAG_PROCESS_THREAD_MESSAGES = 1

● FLAG_PROCESS_THREAD_MESSAGES_PHYSICS = 2

● FLAG_PROCESS_THREAD_MESSAGES_ALL = 3


enum  DuplicateFlags:

● DUPLICATE_SIGNALS = 1
复制该节点的信号。
● DUPLICATE_GROUPS = 2
复制节点的组。
● DUPLICATE_SCRIPTS = 4
复制该节点的脚本。
● DUPLICATE_USE_INSTANTIATION = 8
使用实例化进行复制。

实例与原件保持链接，因此当原件发生变化时，实例也会发生变化。


enum  InternalMode:

● INTERNAL_MODE_DISABLED = 0
该节点不是内部节点。
● INTERNAL_MODE_FRONT = 1
该节点将被放置在父节点的节点列表开头，在所有非内部兄弟节点之前。
● INTERNAL_MODE_BACK = 2
该节点将被放置在父节点的节点列表末尾，在所有非内部兄弟节点之后。


常量
● NOTIFICATION_ENTER_TREE = 10
当该节点进入 SceneTree 时收到的通知。

这个通知会在相关的 tree_entered 之前发出。

● NOTIFICATION_EXIT_TREE = 11
当该节点即将退出 SceneTree 时收到的通知。

这个通知会在相关的 tree_exiting 之后发出。

● NOTIFICATION_MOVED_IN_PARENT = 12   (已废弃)
已废弃。不会再发出这个通知。请改用 NOTIFICATION_CHILD_ORDER_CHANGED。
● NOTIFICATION_READY = 13
当该节点就绪时接收到通知。见 _ready()。
● NOTIFICATION_PAUSED = 14
当该节点被暂停时接收到的通知。
● NOTIFICATION_UNPAUSED = 15
当该节点被取消暂停时收到的通知。
● NOTIFICATION_PHYSICS_PROCESS = 16
当设置了 physics process 标志时，每一帧都会收到的通知（见 set_physics_process()）。
● NOTIFICATION_PROCESS = 17
当设置了 process 标志时，每一帧都会收到的通知（见 set_process()）。
● NOTIFICATION_PARENTED = 18
当一个节点被设置为另一个节点的子节点时收到该通知。

注意：这并不意味着一个节点进入了 SceneTree。

● NOTIFICATION_UNPARENTED = 19
当该节点失去父节点时收到的通知（父节点将其从子节点列表中删除）。
● NOTIFICATION_SCENE_INSTANTIATED = 20
当场景被实例化时，该场景的所有者收到的通知。
● NOTIFICATION_DRAG_BEGIN = 21
当拖拽操作开始时收到的通知。所有节点都会收到此通知，而不仅仅是被拖动的节点。

可以通过拖动提供拖动数据的 Control（见 Control._get_drag_data()），或使用 Control.force_drag() 来触发。

请使用 Viewport.gui_get_drag_data() 获取拖动数据。

● NOTIFICATION_DRAG_END = 22
当拖拽操作结束时收到的通知。

请使用 Viewport.gui_is_drag_successful() 检查拖放是否成功。

● NOTIFICATION_PATH_RENAMED = 23
当该节点或其祖级的名称被更改时收到的通知。当节点从场景树中移除，稍后被添加到另一个父节点时，不会收到此通知。
● NOTIFICATION_CHILD_ORDER_CHANGED = 24
子节点列表发生更改时收到的通知。子节点发生添加、移动、删除时列表会发生更改。
● NOTIFICATION_INTERNAL_PROCESS = 25
当设置了内部处理标志时，每一帧都会收到的通知（见 set_process_internal()）。
● NOTIFICATION_INTERNAL_PHYSICS_PROCESS = 26
当设置了内部物理处理标志时，每一帧都会收到的通知（见 set_physics_process_internal()）。
● NOTIFICATION_POST_ENTER_TREE = 27
当该节点就绪，在收到 NOTIFICATION_READY 之前收到的通知。与后者不同，该节点每次进入树时都会发送，而不是只发送一次。
● NOTIFICATION_DISABLED = 28
当该节点被禁用时收到的通知。见 PROCESS_MODE_DISABLED。
● NOTIFICATION_ENABLED = 29
当该节点被禁用后又再次被启用时收到的通知。见 PROCESS_MODE_DISABLED。
● NOTIFICATION_EDITOR_PRE_SAVE = 9001
在编辑器中保存有节点的场景之前收到的通知。这个通知只在 Godot 编辑器中发送，不会出现在导出的项目中。
● NOTIFICATION_EDITOR_POST_SAVE = 9002
在编辑器中保存有节点的场景后立即收到通知。这个通知只在 Godot 编辑器中发送，在导出的项目中不会出现。
● NOTIFICATION_WM_MOUSE_ENTER = 1002
鼠标进入窗口时收到的通知。

为内嵌窗口实现，并在桌面和 Web 平台上实现。

● NOTIFICATION_WM_MOUSE_EXIT = 1003
鼠标离开窗口时收到的通知。

为内嵌窗口实现，并在桌面和 Web 平台上实现。

● NOTIFICATION_WM_WINDOW_FOCUS_IN = 1004
当该节点的父 Window 获得焦点时收到的通知。可能是在同一引擎实例的两个窗口之间的焦点变化，也可能是从操作系统桌面或第三方应用程序切换到游戏的某个窗口（在这种情况下，还会发出 NOTIFICATION_APPLICATION_FOCUS_IN）。

Window 节点会在获得焦点时收到该通知。

● NOTIFICATION_WM_WINDOW_FOCUS_OUT = 1005
当该节点的父 Window 失去焦点时收到的通知。可能是在同一引擎实例的两个窗口之间的焦点变化，也可能是从游戏的某个窗口切换到操作系统桌面或第三方应用程序（在这种情况下，还会发出 NOTIFICATION_APPLICATION_FOCUS_OUT）。

Window 节点会在失去焦点时收到该通知。

● NOTIFICATION_WM_CLOSE_REQUEST = 1006
当发出关闭请求时，从操作系统收到的通知（例如使用“关闭”按钮或按下 Alt + F4 关闭窗口时）。

在桌面平台上实现。

● NOTIFICATION_WM_GO_BACK_REQUEST = 1007
当发出返回请求时，从操作系统收到的通知（例如在 Android 系统上按下“返回”按钮）。

仅限 Android 平台。

● NOTIFICATION_WM_SIZE_CHANGED = 1008
当窗口大小发生改变时，从操作系统收到的通知。
● NOTIFICATION_WM_DPI_CHANGE = 1009
当屏幕的 DPI 发生更改时，从操作系统受到的通知。仅在 macOS 上实现。
● NOTIFICATION_VP_MOUSE_ENTER = 1010
当鼠标指针进入 Viewport 的可见区域时收到的通知，可见区域指没有被其他 Control 和 Window 遮挡的区域，并且需要 Viewport.gui_disable_input 为 false，与当前是否持有焦点无关。
● NOTIFICATION_VP_MOUSE_EXIT = 1011
当鼠标指针离开 Viewport 的可见区域时收到的通知，可见区域指没有被其他 Control 和 Window 遮挡的区域，并且需要 Viewport.gui_disable_input 为 false，与当前是否持有焦点无关。
● NOTIFICATION_OS_MEMORY_WARNING = 2009
当应用程序超过其分配的内存时，从操作系统收到的通知。

仅限 iOS 平台。

● NOTIFICATION_TRANSLATION_CHANGED = 2010
当翻译可能发生变化时收到的通知。会在用户改变区域设置时触发。可以用来响应语言的变化，例如实时改变 UI 字符串。可配合内置的翻译支持使用，比如 Object.tr()。
● NOTIFICATION_WM_ABOUT = 2011
当发出“关于”信息请求时，从操作系统收到的通知。

仅限 macOS 平台。

● NOTIFICATION_CRASH = 2012
当引擎即将崩溃时，从Godot的崩溃处理程序收到的通知。

如果崩溃处理程序被启用，这只会在桌面平台上实现。

● NOTIFICATION_OS_IME_UPDATE = 2013
当输入法引擎发生更新时，从操作系统收到的通知（例如，IME 光标位置或组成字符串的变化）。

仅限 macOS 平台。

● NOTIFICATION_APPLICATION_RESUMED = 2014
当应用程序恢复时，从操作系统收到的通知。

仅限 Android 平台。

● NOTIFICATION_APPLICATION_PAUSED = 2015
当应用程序暂停时，从操作系统收到的通知。

仅限 Android 平台。

● NOTIFICATION_APPLICATION_FOCUS_IN = 2016
当应用程序获得焦点时从操作系统收到的通知，即焦点将从操作系统桌面或第三方应用程序更改为 Godot 实例的任何一个打开窗口时。

在桌面平台上被实现。

● NOTIFICATION_APPLICATION_FOCUS_OUT = 2017
当应用程序失去焦点时从操作系统收到通知，即焦点将从 Godot 实例的任何一个打开窗口，更改为操作系统桌面或第三方应用程序时。

在桌面平台上被实现。

● NOTIFICATION_TEXT_SERVER_CHANGED = 2018
文本服务器被更改时，收到的通知。

属性说明

● String editor_description [默认： ""]set_editor_description(值) setterget_editor_description() getter

为该节点添加自定义描述。该节点在编辑器的场景树中处于悬停状态时，该描述将显示在工具提示中。


● MultiplayerAPI multiplayerget_multiplayer() getter

与该节点关联的 MultiplayerAPI 实例。见 SceneTree.get_multiplayer()。

注意：将节点重命名或者在树中移动都不会将 MultiplayerAPI 移动至新的路径，你需要手动进行更新。


● StringName nameset_name(值) setterget_name() getter

该节点的名称。这个名称在兄弟节点（来自同一父节点的其他子节点）中是唯一的。当设置为现有名称时，节点将自动重命名。

注意：自动生成的名称可能包含 @ 字符，在使用 add_child() 时保留该字符用于唯一名称。手动设置名称时，将删除任何 @。


● Node ownerset_owner(值) setterget_owner() getter

该节点的所有者。节点的所有者可以是任何祖先节点（即父节点、祖父节点等沿场景树向上的节点）。也就是说，应该在设置所有者之前调用 add_child()，这样才能存在父子关系。（通过 PackedScene）保存节点时，它拥有的所有节点也会随之保存。这样就可以创建复杂的场景树，能够进行实例化和子实例化。

注意：如果想要将子节点持久化进 PackedScene，除了调用 add_child() 之外还必须设置 owner。通常在工具脚本和编辑器插件中会用到。如果将新节点添加到了场景树中但没有将场景树中的祖先设置为所有者，那么这个节点在 2D/3D 视图中可见，但在场景树中不可见（也不会在打包或保存时进行持久化）。


● ProcessMode process_mode [默认： 0]set_process_mode(值) setterget_process_mode() getter

可用于暂停或取消暂停该节点，也可以让该节点根据 SceneTree 来暂停，还可以让它继承父级的处理模式（默认）。


● int process_physics_priority [默认： 0]set_physics_process_priority(值) setterget_physics_process_priority() getter

与 process_priority 类似，但是作用于 NOTIFICATION_PHYSICS_PROCESS、_physics_process() 以及内部版本。


● int process_priority [默认： 0]set_process_priority(值) setterget_process_priority() getter

该节点在已启用的处理回调（即 NOTIFICATION_PROCESS、NOTIFICATION_PHYSICS_PROCESS 及其内部对应物）的执行顺序中的优先级。进程优先级值较低的节点将首先执行其处理回调。


● ProcessThreadGroup process_thread_group [默认： 0]set_process_thread_group(值) setterget_process_thread_group() getter

设置这个节点的处理线程组（基本上就是在主线程还是子线程中接收 NOTIFICATION_PROCESS、NOTIFICATION_PHYSICS_PROCESS、_process()、_physics_process() 以及这些回调的内部版本）。

默认情况下线程组为 PROCESS_THREAD_GROUP_INHERIT，表示这个节点属于和父节点一样的线程组。同一线程组中的节点会一起处理，独立于其他线程组（由 process_thread_group_order 决定）。如果设为 PROCESS_THREAD_GROUP_SUB_THREAD，则该线程组会在子线程（非主线程）中执行，否则设为 PROCESS_THREAD_GROUP_MAIN_THREAD 就会在主线程中处理。如果父节点和先祖节点都没有设置为非继承，则该节点属于默认线程组。默认分组在主线程中处理，分组顺序为 0。

在子线程中处理时，线程组之外的大多数函数都禁止访问（调试模式下会报错）。请使用 Object.call_deferred()、call_thread_safe()、call_deferred_thread_group() 等方法与主线程（或其他线程组）通信。

线程组更好的理解方式是，非 PROCESS_THREAD_GROUP_INHERIT 的节点都会将设为继承的子节点（以及后续子孙节点）纳入它的处理线程组。这样该分组中的节点就会一起处理，包括包含它们的节点。


● int process_thread_group_orderset_process_thread_group_order(值) setterget_process_thread_group_order() getter

修改处理线程组的顺序。顺序取值较小的分组会在较大的分组前处理。例如，可以让大量的节点先在子线程中处理，然后另一组节点要在主线程中获取它们的处理结果。


● BitField[ProcessThreadMessages] process_thread_messagesset_process_thread_messages(值) setterget_process_thread_messages() getter

设置当前线程组是否会在线程中处理消息（调用 call_deferred_thread_group()），以及是否需要在常规处理和物理处理回调中接收消息。


● String scene_file_pathset_scene_file_path(值) setterget_scene_file_path() getter

如果一个场景是从一个文件实例化来的，则其最顶层的节点的 scene_file_path 中，将包含它从何处被加载的绝对文件路径（例如 res://levels/1.tscn）。否则 scene_file_path 被设置为一个空字符串。


● bool unique_name_in_owner [默认： false]set_unique_name_in_owner(值) setteris_unique_name_in_owner() getter

将这个节点的名称设置为其 owner 中的唯一名称。这样就可以从该场景中的任意节点处使用 %名称 来访问这个节点，无需使用完整路径。

如果所有者相同的另一个节点已经将该名称声明为唯一，那么其他节点就无法再将此名称设置为唯一名称。


方法说明

● void _enter_tree() virtual

当节点进入 SceneTree 时调用（例如实例化时，场景改变时，或者在脚本中调用 add_child() 后）。如果节点有子节点，则首先调用它的 _enter_tree() 回调函数，然后再调用子节点的回调函数。

对应于 Object._notification() 中的 NOTIFICATION_ENTER_TREE 通知。


● void _exit_tree() virtual

当节点即将离开 SceneTree 时被调用（例如，在释放、场景改变或在脚本中调用 remove_child() 后）。如果该节点有子节点，它的 _exit_tree() 回调将在所有子节点离开树后被最后调用。

对应于 Object._notification() 中的 NOTIFICATION_EXIT_TREE 通知和 tree_exiting 信号。要在节点已经离开活动树时得到通知，请连接到 tree_exited。


● PackedStringArray _get_configuration_warnings() virtual const

如果覆盖这个方法的脚本是 tool 脚本，那么这个函数所返回的数组中的元素会在“场景”面板中显示为警告。

返回空数组不会生成警告。

这个节点的警告需要更新时，请调用 update_configuration_warnings()。

@export var energy = 0:
    set(value):
        energy = value
        update_configuration_warnings()

func _get_configuration_warnings():
    if energy < 0:
        return ["Energy 必须大于等于 0。"]
    else:
        return []


● void _input(event: InputEvent) virtual

有输入事件时会被调用。输入事件会沿节点树向上传播，直到有节点将其消耗。

只有在启用输入处理时才会被调用，如果该方法被重写则会自动启用，可以使用 set_process_input() 进行切换。

要消耗输入事件，阻止它进一步传播到其他节点，可以调用 Viewport.set_input_as_handled()。

对于游戏输入，_unhandled_input() 和 _unhandled_key_input() 通常更适合，因为它们允许 GUI 首先拦截事件。

注意：仅当该节点存在于场景树中时（即不是孤立节点），此方法才会被调用。


● void _physics_process(delta: float) virtual

在主循环的物理处理步骤中被调用。物理处理意味着帧率与物理同步，即 delta 变量应该是常量。 delta 的单位是秒。

只有当物理处理被启用时才会被调用，如果这个方法被重写，就会自动被调用，并且可以使用 set_physics_process() 进行切换。

对应于 Object._notification() 中的 NOTIFICATION_PHYSICS_PROCESS 通知。

注意：这个方法只有在当节点存在于场景树中时才会被调用（也就是说，如果它不是“孤儿”）。


● void _process(delta: float) virtual

在主循环的处理步骤中被调用。处理发生在每一帧，并且尽可能快，所以从上一帧开始的 delta 时间不是恒定的。delta 的单位是秒。

只有在启用处理的情况下才会被调用，如果这个方法被重写，会自动进行处理，可以用 set_process() 来开关。

对应于 Object._notification() 中的 NOTIFICATION_PROCESS 通知。

注意：这个方法只有在节点存在于场景树中时才会被调用（也就是说，如果它不是“孤儿”）。


● void _ready() virtual

当节点“就绪”时被调用，即当节点及其子节点都已经进入场景树时。如果该节点有子节点，将首先触发子节点的 _ready() 回调，稍后父节点将收到就绪通知。

对应 Object._notification() 中的 NOTIFICATION_READY 通知。另请参阅用于变量的 @onready 注解。

通常用于初始化。对于更早的初始化，可以使用 Object._init()。另见 _enter_tree()。

注意：对于每个节点可能仅调用一次 _ready()。从场景树中移除一个节点后，并再次添加该节点时，将不会第二次调用 _ready()。这时可以通过使用 request_ready()，它可以在再次添加节点之前的任何地方被调用。


● void _shortcut_input(event: InputEvent) virtual

当一个 InputEventKey 或 InputEventShortcut，尚未被 _input() 或任何 GUI Control 项使用时调用。这是在 _unhandled_key_input() 和 _unhandled_input() 之前调用的。输入事件通过节点树向上传播，直到一个节点消耗它。

它仅在启用快捷键处理时调用，如果此方法被覆盖，则会自动调用，并且可以使用 set_process_shortcut_input() 进行开关。

要消耗输入事件，并阻止它进一步传播到其他节点，可以调用 Viewport.set_input_as_handled()。

此方法可用于处理快捷键。如果是常规的 GUI 事件，请改用 _input()。游戏事件通常应该使用 _unhandled_input() 或 _unhandled_key_input() 处理。

注意：仅当该节点存在于场景树中（即它不是一个孤儿节点）时，此方法才会被调用。


● void _unhandled_input(event: InputEvent) virtual

当一个 InputEvent 尚未被 _input() 或任何 GUI Control 项消耗时调用。这是在 _shortcut_input() 和 _unhandled_key_input() 之后调用的。输入事件通过节点树向上传播，直到一个节点消耗它。

只有在未处理的输入处理被启用时，才会被调用，如果该方法被重写，则会自动被调用，并且可以使用 set_process_unhandled_input() 进行切换。

要消耗输入事件，并阻止它进一步传播到其他节点，可以调用 Viewport.set_input_as_handled()。

对于游戏输入，这个方法通常比 _input() 更合适，因为 GUI 事件需要更高的优先级。对于键盘快捷键，请考虑改用 _shortcut_input()，因为是在这个方法之前调用的。最后，如果要处理键盘事件，那么出于性能方面的原因请考虑使用 _unhandled_key_input()。

注意：仅当该节点存在于场景树中（即不是孤儿节点）时，该方法才会被调用。


● void _unhandled_key_input(event: InputEvent) virtual

当 InputEventKey 没有被 _input() 或任何 GUI Control 项目消耗时调用。这是在 _shortcut_input() 之后、_unhandled_input() 之前调用的。输入事件通过节点树向上传播，直到某个节点将其消耗。

只有在启用了未处理按键输入处理时才会被调用，如果覆盖了这个方法就会自动启用，并且可以用 set_process_unhandled_key_input() 来开关。

要消耗输入事件并阻止它进一步传播到其他节点，可以调用 Viewport.set_input_as_handled()。

在处理快捷键后，此方法可用于使用 Alt、Alt + Ctrl 和 Alt + Shift 修饰符处理 Unicode 字符输入。

对于游戏输入，这和 _unhandled_input() 通常比 _input() 更适合，因为应该先处理 GUI 事件。该方法的性能也比 _unhandled_input() 更好，因为 InputEventMouseMotion 等无关事件会被自动过滤。

注意：只有当节点存在于场景树中（即不是孤儿节点）时，该方法才会被调用。


● void add_child(node: Node, force_readable_name: bool = false, internal: InternalMode = 0)

将 node 添加为子节点。节点可以有任意数量的子节点，但子节点的名称必须唯一。删除父节点时会自动删除子节点，因此可以通过删除最顶层的节点来删除整个场景。

如果 force_readable_name 为 true，则将提高所添加的 node 的可读性。如果尚未命名，node 将重命名为它的类型，如果存在 name 相同的兄弟节点，则会添加合适的数字后缀。这个操作很慢。因此，建议将其保留为 false，在这两种情况下会分配包含 @ 的虚拟名称。

如果 internal 不同于 INTERNAL_MODE_DISABLED，则该子节点将被添加为内部节点。get_children() 等方法会忽略这种节点，除非它们的参数 include_internal 为 true。这种功能的设计初衷是对用户隐藏内部节点，这样用户就不会意外删除或修改这些节点。部分 GUI 节点会使用这个功能，例如 ColorPicker。可用的模式见 InternalMode。

注意：如果子节点已经有父节点，则该函数会失败。请先使用 remove_child() 将节点从当前父节点中移除。例如：

var child_node = get_child(0)
if child_node.get_parent():
    child_node.get_parent().remove_child(child_node)
add_child(child_node)

如果你需要将子节点添加到子节点列表中特定节点的下方，请使用 add_sibling() 而不是该方法。

注意：如果想让子节点持久化到某个 PackedScene 的，除了调用 add_child() 之外，还必须设置 owner。通常在工具脚本和编辑器插件中会用到。如果在没有设置 owner，只调用了 add_child()，则新添加的 Node 在场景树中将不可见，但在 2D/3D 视图中却是可见的。


● void add_sibling(sibling: Node, force_readable_name: bool = false)

将一个 sibling 节点添加到当前节点的父节点，与该节点处于同一级别，就在它的正下方。

如果 force_readable_name 为 true，则提高添加的 sibling 的可读性。如果没有命名，sibling 将被重命名为它的类型，如果它与一个同级节点共享 name，则添加一个更合适的数字后缀。这个操作很慢。因此，建议将其保留为 false，这会在两种情况下分配一个以 @ 为特色的虚拟名称。

如果不需要将该子节点添加到子列表中特定节点的下方，请使用 add_child() 而不是该方法。

注意：如果这个节点是内部的，则新的同级节点也将是内部的（参见 add_child() 中的 internal 参数）。


● void add_to_group(group: StringName, persistent: bool = false)

将节点添加到一个组中。组是命名和组织节点子集的辅助工具，例如“敌人”或“收集品”等。一个节点可以在任意数量的组中。节点可以随时被分配到一个组中，但在它们进入场景树之前不会添加（参见 is_inside_tree()）。参阅描述中的注释，以及 SceneTree 中的分组方法。

persistent 选项在将节点打包到 PackedScene 并保存到文件时使用。非持久化的组不会被存储。

注意：出于性能原因，不保证节点组的顺序。不应依赖节点组的顺序，因为它可能因项目运行而异。


● Variant call_deferred_thread_group(method: StringName, ...) vararg

这个函数类似于 Object.call_deferred()，但是会在处理节点线程组时进行调用。如果节点线程组在子线程中处理，那么调用就会在该线程中进行，时机为 NOTIFICATION_PROCESS 和 NOTIFICATION_PHYSICS_PROCESS、_process() 和 _physics_process()，或者对应的内部版本之前。


● Variant call_thread_safe(method: StringName, ...) vararg

这个函数能够确保调用成功，无论是否从线程中调用。如果是从不允许调用该函数的线程中调用的，那么调用就会变成延迟调用。否则就会直接调用。


● bool can_process() const

如果场景树暂停时节点可以处理（请参阅 process_mode），则返回 true。如果场景树未被暂停，则始终返回 true，如果该节点不在树中，则始终返回 false。


● Tween create_tween()

新建 Tween 并将其绑定到这个节点。与如下操作等价：

get_tree().create_tween().bind_node(self)

该 Tween 将在下一个处理帧或物理处理帧时自动开始（取决于 Tween.TweenProcessMode）。


● Node duplicate(flags: int = 15) const

复制该节点，返回一个新的节点。

可以使用 flags 微调该行为（请参阅 DuplicateFlags）。

注意：如果节点包含一个带有构造参数的脚本（即需要向 Object._init() 方法提供参数），它将无法正常工作。在这种情况下，节点将在没有脚本的情况下被复制。


● Node find_child(pattern: String, recursive: bool = true, owned: bool = true) const

查找此节点的后代中，其名称与 String.match() 中的 pattern 匹配的第一个节点。也会查找内部子节点（见 add_child() 的 internal 参数）。

pattern 不匹配完整路径，只匹配单个节点名称。它区分大小写，"*" 匹配零个或多个字符，"?" 匹配除 "." 之外的任意单个字符。

如果 recursive 为 true，则查找范围包括所有子节点，即使嵌套很深。节点按树顺序检查，因此首先检查该节点的第一个直接子节点，然后是该直接子节点的直接子节点，等等，然后移动到第二个直接子节点，依此类推。如果 recursive 为 false，则仅匹配该节点的直接子节点。

如果 owned 为 true，则该方法仅查找分配有 Node.owner 的节点。这对于通过脚本实例化的场景尤其重要，因为这些场景没有所有者。

如果找不到匹配的 Node，则返回 null。

注意：由于该方法会遍历节点的所有后代，因此它是获取对另一个节点的引用的最慢方法。只要有可能，请考虑改用使用唯一名称的 get_node()（请参阅 unique_name_in_owner），或将该节点引用缓存到变量中。

注意：要查找匹配一个模式或类类型的所有后代节点，请参阅 find_children()。


● Array[Node] find_children(pattern: String, type: String = "", recursive: bool = true, owned: bool = true) const

查找该节点的后代节点，其名称与 String.match() 中的 pattern 匹配，和/或类型与 Object.is_class() 中的 type 匹配。也会查找内部子节点（见 add_child() 的 internal 参数）。

pattern 不匹配完整路径，只匹配单个节点名称。它区分大小写，"*" 匹配零个或多个字符，"?" 匹配除 "." 之外的任意单个字符。

type 将检查相等性或继承关系，并且区分大小写。"Object" 会匹配类型为 "Node" 的节点，但反之则不然。

如果 recursive 为 true，则匹配范围包括所有子节点，即使嵌套很深。节点按树顺序检查，因此首先检查该节点的第一个直接子节点，然后是该直接子节点的直接子节点，等等，然后移动到第二个直接子节点，依此类推。如果 recursive 为 false，则仅匹配该节点的直接子节点。

如果 owned 为 true，则该方法仅查找分配有 Node.owner 的节点。这对于通过脚本实例化的场景尤其重要，因为这些场景没有所有者。

如果找不到匹配的节点，则返回空数组。

注意：由于该方法会遍历节点的所有后代，因此它是获取对其他节点的引用的最慢方法。只要有可能，请考虑将节点引用缓存到变量中。

注意：如果只想查找匹配模式的第一个后代节点，请参阅 find_child()。


● Node find_parent(pattern: String) const

查找当前节点的第一个父节点，其名称与 String.match() 中的 pattern 匹配。

pattern 不匹配完整路径，只匹配单个节点名称。它区分大小写，"*" 匹配零个或多个字符，"?" 匹配除 "." 之外的任意单个字符。

注意：由于该方法在场景树中向上遍历，因此在大型、深度嵌套的场景树中可能会很慢。只要有可能，请考虑使用具有唯一名称的 get_node()（请参阅 unique_name_in_owner），或将该节点引用缓存到变量中。


● Node get_child(idx: int, include_internal: bool = false) const

按索引返回一个子节点（见 get_child_count()）。这个方法经常被用于遍历一个节点的所有子节点。

负索引将从最后一个开始访问子节点。

如果 include_internal 为 false，则跳过内部子节点（见 add_child() 中的 internal 参数）。

要通过名称访问一个子节点，请使用 get_node()。


● int get_child_count(include_internal: bool = false) const

返回子节点的数量。

如果 include_internal 为 false ，则不计算内部子节点（见 add_child() 的 internal 参数）。


● Array[Node] get_children(include_internal: bool = false) const

返回一组对节点子节点的引用。

如果 include_internal 为 false，则返回的数组将不包含内部子节点（见 add_child() 中的 internal 参数）。


● Array[StringName] get_groups() const

返回一个列出该节点所属的分组的数组。

注意：出于性能原因，不保证节点分组的顺序。 不应依赖节点分组的顺序，因为它可能因项目运行而异。

注意：引擎在内部使用了一些分组名称（全部以下划线开头）。为避免与内部分组冲突，请勿添加名称以下划线开头的自定义分组。要在遍历 get_groups() 时排除内部分组，请使用以下代码段：

==仅存储节点的非内部分组（作为一个字符串数组）。==

var non_internal_groups = []
for group in get_groups():
    if not group.begins_with("_"):
        non_internal_groups.push_back(group)


● int get_index(include_internal: bool = false) const

返回节点在场景树分支中的顺序。例如，如果在第一个子节点上调用，则位置为 0。

如果 include_internal 为 false，则索引将不会考虑内部子节点，即第一个非内部子节点的索引将为 0（见 add_child() 中的 internal 参数）。


● Window get_last_exclusive_window() const

返回包含该节点的 Window，或者是从包含该节点的窗口开始的窗口链中最近的独占子项。


● int get_multiplayer_authority() const

返回这个节点多人游戏控制者的对等体 ID。见 set_multiplayer_authority()。


● Node get_node(path: NodePath) const

获取一个节点。NodePath 可以是到一个节点的相对路径（从当前节点）或绝对路径（在场景树中）。如果该路径不存在，则返回 null 并记录一个错误。尝试访问该返回值上的方法，将产生一个“尝试在一个 null 实例上调用 <method>。”错误。

注意：获取绝对路径，仅在节点位于场景树内部时有效（参见 is_inside_tree()）。

示例：假设你当前的节点是 Character 和以下树：

/root
/root/Character
/root/Character/Sword
/root/Character/Backpack/Dagger
/root/MyGame
/root/Swamp/Alligator
/root/Swamp/Mosquito
/root/Swamp/Goblin

可能的路径有：

get_node("Sword")
get_node("Backpack/Dagger")
get_node("../Swamp/Alligator")
get_node("/root/MyGame")


● Array get_node_and_resource(path: NodePath)

按照 NodePath 的子名称（例如 Area2D/CollisionShape2D:shape）指定的方式，获取节点及其一个资源。如果在 NodePath 中指定了多个嵌套资源，则将获取最后一个。

返回值是一个大小为 3 的数组：第一个索引指向该 Node（如果未找到，则为 null），第二个索引指向 Resource（或者未找到时为 null），第三个索引是剩余的 NodePath，如果有的话。

例如，假设 Area2D/CollisionShape2D 是一个有效的节点，并且它的 shape 属性已被分配了一个 RectangleShape2D 资源，那么可以得到这样的输出：

print(get_node_and_resource("Area2D/CollisionShape2D")) # [[CollisionShape2D:1161], Null, ]
print(get_node_and_resource("Area2D/CollisionShape2D:shape")) # [[CollisionShape2D:1161], [RectangleShape2D:1156], ]
print(get_node_and_resource("Area2D/CollisionShape2D:shape:extents")) # [[CollisionShape2D:1161], [RectangleShape2D:1156], :extents]


● Node get_node_or_null(path: NodePath) const

类似于 get_node()，但在 path 没有指向有效的 Node 时不会记录错误。


● Node get_parent() const

返回当前节点的父节点，如果节点缺少父节点，则返回 null。


● NodePath get_path() const

返回当前节点的绝对路径。这只在当前节点在场景树中起作用（见 is_inside_tree()）。


● NodePath get_path_to(node: Node, use_unique_path: bool = false) const

返回从该节点到指定节点 node 的相对 NodePath。这两个节点都必须在同一个场景中，否则函数会失败。

如果 use_unique_path 为 true，则会返回考虑唯一节点的最短路径。

注意：如果你获取了从唯一节点开始的相对路径，则该路径可能由于唯一节点的名称长度而比普通的相对路径长。


● float get_physics_process_delta_time() const

返回自上一个物理绑定帧以来经过的时间（单位为秒）（见 _physics_process()）。除非通过 Engine.physics_ticks_per_second 更改每秒帧数，否则这在物理处理中始终是一个恒定值。


● float get_process_delta_time() const

返回自上次处理回调以来经过的时间（单位为秒）。这个值可能因帧而异。


● bool get_scene_instance_load_placeholder() const

如果这是一个实例加载占位符，则返回 true。见 InstancePlaceholder。


● SceneTree get_tree() const

返回包含该节点的 SceneTree。如果该节点不在场景树内，则返回 null 并打印错误。另见 is_inside_tree()。


● String get_tree_string()

将树以 String 的形式返回。主要用于调试。这个版本显示相对于当前节点的路径，适合复制/粘贴到 get_node() 函数中。也可以用于游戏中的 UI/UX。

示例输出：

TheGame
TheGame/Menu
TheGame/Menu/Label
TheGame/Menu/Camera2D
TheGame/SplashScreen
TheGame/SplashScreen/Camera2D


● String get_tree_string_pretty()

类似于 get_tree_string()，会将树以 String 的形式返回。这个版本使用的是一种更加图形化的呈现方式，类似于在“场景”面板中显示的内容。非常适合检查较大的树。

输出示例：

 ┖╴TheGame
    ┠╴Menu
    ┃  ┠╴Label
    ┃  ┖╴Camera2D
    ┖╴SplashScreen
       ┖╴Camera2D


● Viewport get_viewport() const

返回节点的 Viewport。


● Window get_window() const

返回包含该节点的 Window。如果该节点在主窗口中，则相当于获取根节点（get_tree().get_root()）。


● bool has_node(path: NodePath) const

如果 NodePath 指向的节点存在，则返回 true。


● bool has_node_and_resource(path: NodePath) const

如果 NodePath 指向一个有效的节点，并且它的子名称指向一个有效的资源，例如 Area2D/CollisionShape2D:shape，则返回 true。具有非 Resource 类型的属性（例如节点或基本数学类型）不被认为是资源。


● bool is_ancestor_of(node: Node) const

如果给定节点是当前节点的直接或间接子节点，则返回 true。


● bool is_displayed_folded() const

如果该节点在“场景”面板中被折叠，则返回 true。该方法仅适用于编辑器工具。


● bool is_editable_instance(node: Node) const

如果 node 有与相对于此节点的可编辑子节点，则返回 true。该方法仅适用于编辑器工具。


● bool is_greater_than(node: Node) const

如果给定节点在场景层次结构中出现的时间晚于当前节点，则返回 true。


● bool is_in_group(group: StringName) const

如果该节点在指定的组中，则返回 true。参阅描述中的注释和 SceneTree 中的组方法。


● bool is_inside_tree() const

如果该节点当前在 SceneTree 中，返回 true。


● bool is_multiplayer_authority() const

如果本地系统为这个节点的多人游戏控制者，则返回 true。


● bool is_node_ready() const

如果该节点已就绪，则返回 true，即该节点位于场景树中，并且所有子项均已初始化。

request_ready() 会将其重置回 false。


● bool is_physics_processing() const

如果启用了物理处理，返回 true（见 set_physics_process()）。


● bool is_physics_processing_internal() const

如果内部物理处理被启用，返回 true（见 set_physics_process_internal()）。


● bool is_processing() const

如果开启了处理，返回 true（见 set_process()）。


● bool is_processing_input() const

如果节点正在处理输入，则返回 true（见 set_process_input()）。


● bool is_processing_internal() const

如果启用了内部处理，则返回 true（见 set_process_internal()）。


● bool is_processing_shortcut_input() const

如果节点正在处理快捷键，则返回 true（见 set_process_shortcut_input()）。


● bool is_processing_unhandled_input() const

如果节点正在处理未被处理的输入，则返回 true（见 set_process_unhandled_input()）。


● bool is_processing_unhandled_key_input() const

如果节点正在处理未被处理的键输入，则返回 true（见 set_process_unhandled_key_input()）。


● void move_child(child_node: Node, to_index: int)

在其他子节点中将子节点移动到不同的索引（顺序）。由于调用、信号等是按树顺序执行的，因此更改子节点的顺序可能会很有用。如果 to_index 为负数，索引将从末尾开始计算。

注意：内部子节点只能在其期望的“内部范围”内移动（参见 add_child() 中的 internal 参数）。


● void notify_deferred_thread_group(what: int)

类似于 call_deferred_thread_group()，但针对的是通知。


● void notify_thread_safe(what: int)

类似于 call_thread_safe()，但针对的是通知。


● void print_orphan_nodes() static

输出所有孤立节点（SceneTree 之外的节点）。用于调试。

注意：print_orphan_nodes() 只在调试版本中有效。在以发布模式导出的项目中调用时，print_orphan_nodes() 不会输出任何内容。


● void print_tree()

将树打印到标准输出。主要用于调试。这个版本显示相对于当前节点的路径，适合复制/粘贴到 get_node() 函数中。

示例输出：

TheGame
TheGame/Menu
TheGame/Menu/Label
TheGame/Menu/Camera2D
TheGame/SplashScreen
TheGame/SplashScreen/Camera2D


● void print_tree_pretty()

类似于 print_tree()，会将树打印到标准输出。这个版本显示了一种更加图形化的表示方式，类似于在场景面板中显示的内容。非常适合检查较大的树。

输出示例：

 ┖╴TheGame
    ┠╴Menu
    ┃  ┠╴Label
    ┃  ┖╴Camera2D
    ┖╴SplashScreen
       ┖╴Camera2D


● void propagate_call(method: StringName, args: Array = [], parent_first: bool = false)

在该节点上并递归地在其所有子节点上，使用 args 中给出的参数调用给定方法（如果存在）。如果 parent_first 参数为 true，则该方法将首先在当前节点上调用，然后在其所有子节点上调用。如果 parent_first 为 false，则子节点上的方法将首先被调用。


● void propagate_notification(what: int)

通过对所有节点调用 Object.notification()，递归地通知当前节点和它的所有子节点。


● void queue_free()

将节点加入队列，在当前帧结束时删除。节点被删除时，它的所有子节点也将被删除，对该节点及其子节点的引用也会失效，见 Object.free()。

同一帧可以对同一个节点调用多次 queue_free()，也可以 Object.free() 已经排队删除的节点。请使用 Object.is_queued_for_deletion() 检查节点是否将在帧结束时被删除。

该节点会在所有其他已延迟的调用结束后释放，所以使用 queue_free() 并不总是和通过 Object.call_deferred() 调用 Object.free() 相同。


● void remove_child(node: Node)

删除一个子节点。该节点不会被删除，必须手动删除。

注意：如果该 owner 不再是父节点或祖先，则该函数可以将被移除节点（或其后代）的 owner 设置为 null。


● void remove_from_group(group: StringName)

从 group 中移除一个节点。如果该节点不在 group 中，则不执行任何操作。见描述中的注意项，以及 SceneTree 中的分组方法。


● void reparent(new_parent: Node, keep_global_transform: bool = true)

将这个 Node 的父节点更改为 new_parent。该节点需要拥有父节点。

如果 keep_global_transform 为 true，则会在支持时保持该节点的全局变换。Node2D、Node3D、Control 支持这个参数（但 Control 只会保留位置）。


● void replace_by(node: Node, keep_groups: bool = false)

将场景中的某个节点替换为给定的节点。经过该节点的订阅会丢失。

如果 keep_groups 为 true，则 node 被添加到被替换节点所在的相同分组中。

注意：给定的节点将成为被替换节点的所有子节点的新的父节点。

注意：被替换的节点不会被自动释放，因此需要将其保存在变量中以备后用，或者使用 Object.free() 释放它。


● void request_ready()

请求再次调用 _ready()。注意，该方法不会被立即调用，而是被安排在该节点再次被添加到场景树时。只会为进行了请求的节点调用 _ready()，也就是说，如果你想让每个子节点都调用 _ready()，就需要为它们分别进行就绪请求（在这种情况下，_ready() 的调用顺序与正常情况下相同）。


● Error rpc(method: StringName, ...) vararg

将给定 method 的远程过程调用请求发送到网络（和本地）上的对等体，可选择将所有其他参数作为参数发送给 RPC 调用的方法。调用请求只会被具有相同 NodePath 的节点接收，该节点包括完全相同的节点名称。行为取决于给定方法的 RPC 配置，请参阅 rpc_config() 和 @GDScript.@rpc。默认情况下，方法不会暴露给 RPC。返回 null。

注意：只有在收到来自 MultiplayerAPI 的 connected_to_server 信号后，才能在客户端上安全地使用 RPC。还需要跟踪连接状态，可通过 MultiplayerAPI 信号（例如 server_disconnected）或检查 get_multiplayer().peer.get_connection_status() == CONNECTION_CONNECTED 来跟踪。


● void rpc_config(method: StringName, config: Variant)

将给定方法 method 的 RPC 模式更改为给定的配置 config，该配置应该是 null（表示禁用）或者是以下形式的 Dictionary：

{
    rpc_mode = MultiplayerAPI.RPCMode,
    transfer_mode = MultiplayerPeer.TransferMode,
    call_local = false,
    channel = 0,
}

见 MultiplayerAPI.RPCMode 和 MultiplayerPeer.TransferMode。另一种选择是使用相应的 @GDScript.@rpc 注解对方法和属性进行注解（例如 @rpc("any_peer")、@rpc("authority")）。默认情况下，方法不会被暴露给网络（和 RPC）。


● Error rpc_id(peer_id: int, method: StringName, ...) vararg

向指定的对等体发送 rpc()，对等体由 peer_id 标识（见 MultiplayerPeer.set_target_peer()）。返回 null。


● void set_deferred_thread_group(property: StringName, value: Variant)

类似于 call_deferred_thread_group()，但针对的是设置属性。


● void set_display_folded(fold: bool)

设置该节点在“场景”面板中的折叠状态。这个方法仅适用于编辑器工具。


● void set_editable_instance(node: Node, is_editable: bool)

设置 node 相对于这个节点的可编辑子节点状态。这个方法仅适用于编辑器工具。


● void set_multiplayer_authority(id: int, recursive: bool = true)

将该节点的多人游戏控制方设置为具有给定对等体 ID 的对等体。多人游戏控制方是对网络上的节点具有控制权限的对等体。可以与 rpc_config() 和 MultiplayerAPI 结合使用。默认为对等体 ID 1（服务器）。如果 recursive，则给定的对等体会被递归设置为该节点所有子节点的控制方。

警告：这样做不会自动将新的控制方复制给其他对等体。开发者需要自己负责。你可以使用 MultiplayerSpawner.spawn_function、RPC、MultiplayerSynchronizer 等方法将这个信息传播出去。另外，父节点的控制方不会传播给新添加的子节点。


● void set_physics_process(enable: bool)

启用或禁用物理（即固定帧率）处理。当一个节点正在被处理时，它会在一个固定的（通常是 60 FPS，参见 Engine.physics_ticks_per_second 以更改）时间间隔，接收一个 NOTIFICATION_PHYSICS_PROCESS （如果存在 _physics_process() 回调，该回调将被调用）。如果 _physics_process() 被重写，则自动被启用。在 _ready() 之前对该函数的任何调用，都将被忽略。


● void set_physics_process_internal(enable: bool)

启用或禁用该节点的内部物理。内部物理处理与正常的 _physics_process() 调用隔离进行，并且由某些节点内部使用，以确保正常工作，即使节点暂停或物理处理因脚本而禁用（set_physics_process()）。仅适用于用于操纵内置节点行为的高级用途。

警告：内置节点依靠内部处理来实现自己的逻辑，所以从你的代码中改变这个值可能会导致意外的行为。为特定的高级用途提供了对此内部逻辑的脚本访问，但不安全且不支持。


● void set_process(enable: bool)

启用或禁用帧处理。当一个节点被处理时，它将在每个绘制的帧上收到一个NOTIFICATION_PROCESS（如果存在，_process()回调将被调用）。如果_process()被重写，则自动启用。在 _ready() 之前对它的任何调用都将被忽略。


● void set_process_input(enable: bool)

启用或禁用输入处理。对于 GUI 控件来说不是必需的。如果 _input() 被覆盖，则自动启用。任何在 _ready() 之前对它的调用都将被忽略。


● void set_process_internal(enable: bool)

启用或禁用此节点的内部处理。内部处理与正常的 _process() 调用隔离进行，并且由某些节点内部使用，以确保正常工作，即使节点已暂停或处理因脚本而禁用（set_process()）。仅适用于操纵内置节点行为的高级用途。

警告：内置节点依赖于内部处理来实现自己的逻辑，因此更改代码中的这个值可能会导致意外行为。为特定的高级用途提供了对此内部逻辑的脚本访问，但不安全且不支持。


● void set_process_shortcut_input(enable: bool)

启用快捷键处理。如果 _shortcut_input() 被覆盖，则自动启用。在 _ready() 之前对此的任何调用都将被忽略。


● void set_process_unhandled_input(enable: bool)

启用未处理的输入处理。这对 GUI 控件来说是不需要的！它使节点能够接收所有以前没有处理的输入（通常是由 Control 处理的）。如果 _unhandled_input() 被覆盖，则自动启用。在 _ready() 之前对它的任何调用都将被忽略。


● void set_process_unhandled_key_input(enable: bool)

启用未处理的按键输入处理。如果 _unhandled_key_input() 被重写，则自动启用。任何在 _ready() 之前对它的调用都将被忽略。


● void set_scene_instance_load_placeholder(load_placeholder: bool)

设置这是否是实例加载占位符。见 InstancePlaceholder。


● void set_thread_safe(property: StringName, value: Variant)

类似于 call_thread_safe()，但用于设置属性。


● void update_configuration_warnings()

更新在场景面板中为该节点显示的警告。

使用 _get_configuration_warnings() 配置要显示的警告消息。

---

---

## 类：   CanvasItem

继承：   Node <   Object
派生：   Control ,   Node2D


2D 空间中所有对象的抽象基类。


描述

2D 空间中所有对象的抽象基类。画布项目以树状排列；子节点继承并扩展其父节点的变换。CanvasItem 由 Control 扩展为 GUI 相关的节点，由 Node2D 扩展为 2D 游戏对象。

任何 CanvasItem 都可以进行绘图。绘图时，引擎会调用 queue_redraw()，然后 NOTIFICATION_DRAW 就会在空闲时被接收到以请求重绘。因此，画布项目不需要每一帧都重绘，这显著提升了性能。这个类还提供了几个用于在 CanvasItem 上绘图的函数（见 draw_* 函数）。不过这些函数都只能在 _draw() 及其对应的 Object._notification() 或连接到 draw 的方法内使用。

画布项目在其画布层上是按树状顺序绘制的。默认情况下，子项目位于其父项目的上方，因此根 CanvasItem 将被画在所有项目的后面。这种行为可以针对每个画布项目进行更改。

CanvasItem 可以隐藏，隐藏时也会隐藏其子项目。通过调整 CanvasItem 的各种其它属性，你还可以调制它的颜色（通过 modulate 或 self_modulate）、更改 Z 索引、混合模式等。


在线教程
Viewport 和画布变换
2D 中的自定义绘图
音频频谱演示


属性
ClipChildrenModeclip_children [默认： 0]intlight_mask [默认： 1]MaterialmaterialColormodulate [默认： Color(1, 1, 1, 1)]Colorself_modulate [默认： Color(1, 1, 1, 1)]boolshow_behind_parent [默认： false]TextureFiltertexture_filter [默认： 0]TextureRepeattexture_repeat [默认： 0]booltop_level [默认： false]booluse_parent_material [默认： false]intvisibility_layer [默认： 1]boolvisible [默认： true]booly_sort_enabled [默认： false]boolz_as_relative [默认： true]intz_index [默认： 0]

方法
void_draw() virtualvoiddraw_animation_slice(animation_length: float, slice_begin: float, slice_end: float, offset: float = 0.0)voiddraw_arc(center: Vector2, radius: float, start_angle: float, end_angle: float, point_count: int, color: Color, width: float = -1.0, antialiased: bool = false)voiddraw_char(font: Font, pos: Vector2, char: String, font_size: int = 16, modulate: Color = Color(1, 1, 1, 1)) constvoiddraw_char_outline(font: Font, pos: Vector2, char: String, font_size: int = 16, size: int = -1, modulate: Color = Color(1, 1, 1, 1)) constvoiddraw_circle(position: Vector2, radius: float, color: Color)voiddraw_colored_polygon(points: PackedVector2Array, color: Color, uvs: PackedVector2Array = PackedVector2Array(), texture: Texture2D = null)voiddraw_dashed_line(from: Vector2, to: Vector2, color: Color, width: float = -1.0, dash: float = 2.0, aligned: bool = true)voiddraw_end_animation()voiddraw_lcd_texture_rect_region(texture: Texture2D, rect: Rect2, src_rect: Rect2, modulate: Color = Color(1, 1, 1, 1))voiddraw_line(from: Vector2, to: Vector2, color: Color, width: float = -1.0, antialiased: bool = false)voiddraw_mesh(mesh: Mesh, texture: Texture2D, transform: Transform2D = Transform2D(1, 0, 0, 1, 0, 0), modulate: Color = Color(1, 1, 1, 1))voiddraw_msdf_texture_rect_region(texture: Texture2D, rect: Rect2, src_rect: Rect2, modulate: Color = Color(1, 1, 1, 1), outline: float = 0.0, pixel_range: float = 4.0, scale: float = 1.0)voiddraw_multiline(points: PackedVector2Array, color: Color, width: float = -1.0)voiddraw_multiline_colors(points: PackedVector2Array, colors: PackedColorArray, width: float = -1.0)voiddraw_multiline_string(font: Font, pos: Vector2, text: String, alignment: HorizontalAlignment = 0, width: float = -1, font_size: int = 16, max_lines: int = -1, modulate: Color = Color(1, 1, 1, 1), brk_flags: BitField[TextServer.LineBreakFlag] = 3, justification_flags: BitField[TextServer.JustificationFlag] = 3, direction: TextServer.Direction = 0, orientation: TextServer.Orientation = 0) constvoiddraw_multiline_string_outline(font: Font, pos: Vector2, text: String, alignment: HorizontalAlignment = 0, width: float = -1, font_size: int = 16, max_lines: int = -1, size: int = 1, modulate: Color = Color(1, 1, 1, 1), brk_flags: BitField[TextServer.LineBreakFlag] = 3, justification_flags: BitField[TextServer.JustificationFlag] = 3, direction: TextServer.Direction = 0, orientation: TextServer.Orientation = 0) constvoiddraw_multimesh(multimesh: MultiMesh, texture: Texture2D)voiddraw_polygon(points: PackedVector2Array, colors: PackedColorArray, uvs: PackedVector2Array = PackedVector2Array(), texture: Texture2D = null)voiddraw_polyline(points: PackedVector2Array, color: Color, width: float = -1.0, antialiased: bool = false)voiddraw_polyline_colors(points: PackedVector2Array, colors: PackedColorArray, width: float = -1.0, antialiased: bool = false)voiddraw_primitive(points: PackedVector2Array, colors: PackedColorArray, uvs: PackedVector2Array, texture: Texture2D = null)voiddraw_rect(rect: Rect2, color: Color, filled: bool = true, width: float = -1.0)voiddraw_set_transform(position: Vector2, rotation: float = 0.0, scale: Vector2 = Vector2(1, 1))voiddraw_set_transform_matrix(xform: Transform2D)voiddraw_string(font: Font, pos: Vector2, text: String, alignment: HorizontalAlignment = 0, width: float = -1, font_size: int = 16, modulate: Color = Color(1, 1, 1, 1), justification_flags: BitField[TextServer.JustificationFlag] = 3, direction: TextServer.Direction = 0, orientation: TextServer.Orientation = 0) constvoiddraw_string_outline(font: Font, pos: Vector2, text: String, alignment: HorizontalAlignment = 0, width: float = -1, font_size: int = 16, size: int = 1, modulate: Color = Color(1, 1, 1, 1), justification_flags: BitField[TextServer.JustificationFlag] = 3, direction: TextServer.Direction = 0, orientation: TextServer.Orientation = 0) constvoiddraw_style_box(style_box: StyleBox, rect: Rect2)voiddraw_texture(texture: Texture2D, position: Vector2, modulate: Color = Color(1, 1, 1, 1))voiddraw_texture_rect(texture: Texture2D, rect: Rect2, tile: bool, modulate: Color = Color(1, 1, 1, 1), transpose: bool = false)voiddraw_texture_rect_region(texture: Texture2D, rect: Rect2, src_rect: Rect2, modulate: Color = Color(1, 1, 1, 1), transpose: bool = false, clip_uv: bool = true)voidforce_update_transform()RIDget_canvas() constRIDget_canvas_item() constTransform2Dget_canvas_transform() constVector2get_global_mouse_position() constTransform2Dget_global_transform() constTransform2Dget_global_transform_with_canvas() constVector2get_local_mouse_position() constTransform2Dget_screen_transform() constTransform2Dget_transform() constRect2get_viewport_rect() constTransform2Dget_viewport_transform() constboolget_visibility_layer_bit(layer: int) constWorld2Dget_world_2d() constvoidhide()boolis_local_transform_notification_enabled() constboolis_transform_notification_enabled() constboolis_visible_in_tree() constVector2make_canvas_position_local(screen_point: Vector2) constInputEventmake_input_local(event: InputEvent) constvoidmove_to_front()voidqueue_redraw()voidset_notify_local_transform(enable: bool)voidset_notify_transform(enable: bool)voidset_visibility_layer_bit(layer: int, enabled: bool)voidshow()

信号

● draw()当该 CanvasItem 必须重绘时发出，发生在相关的 NOTIFICATION_DRAW 通知之后，调用 _draw() 之前。

注意：延迟连接无法使用 draw_* 方法进行绘制。

● hidden()当隐藏时发出。

● item_rect_changed()当 CanvasItem 的 Rect2 边界（位置或大小）发生变化时，或者当发生可能影响这些边界的操作（例如，更改 Sprite2D.texture）时发出。

● visibility_changed()当可见性（隐藏/可见）更改时发出。


枚举
enum  TextureFilter:

● TEXTURE_FILTER_PARENT_NODE = 0
该 CanvasItem 将从其父级继承过滤器。
● TEXTURE_FILTER_NEAREST = 1
纹理过滤仅从最近的像素读取。这使得纹理从近距离看是像素化的，从远处看是颗粒状的（由于多级渐远纹理没有被采样）。
● TEXTURE_FILTER_LINEAR = 2
纹理过滤在最近的 4 个像素之间进行混合。这使得纹理从近处看起来很平滑，从远处看起来却有颗粒感（由于多级渐远纹理没有被采样）。
● TEXTURE_FILTER_NEAREST_WITH_MIPMAPS = 3
纹理过滤从最近的像素读取并在最近的 2 个多级渐远纹理之间进行混合（或者如果 ProjectSettings.rendering/textures/default_filters/use_nearest_mipmap_filter 为 true，则使用最近的多级渐远纹理）。这使得纹理从近处看起来像素化，从远处看起来平滑。

将此用于可能以低缩放查看的非像素艺术纹理（例如，由于 Camera2D 缩放或精灵缩放），因为多级渐远纹理对于平滑小于屏幕像素的像素很重要。

● TEXTURE_FILTER_LINEAR_WITH_MIPMAPS = 4
纹理过滤在最近的 4 个像素和最近的 2 个多级渐远纹理之间进行混合（或者如果 ProjectSettings.rendering/textures/default_filters/use_nearest_mipmap_filter 为 true，则使用最近的多级渐远纹理）。这使得纹理从近处看起来平滑，从远处看起来也平滑。

将此用于可能以低缩放查看的非像素艺术纹理（例如，由于 Camera2D 缩放或精灵缩放），因为多级渐远纹理对于平滑小于屏幕像素的像素很重要。

● TEXTURE_FILTER_NEAREST_WITH_MIPMAPS_ANISOTROPIC = 5
纹理过滤从最近的像素读取并根据表面和相机视图之间的角度在 2 个多级渐远纹理之间进行混合（或者如果 ProjectSettings.rendering/textures/default_filters/use_nearest_mipmap_filter 为 true，则使用最近的多级渐远纹理）。这使得纹理从近处看起来像素化，从远处看起来平滑。各向异性过滤提高了几乎与相机位于一条线的表面上的纹理质量，但速度稍慢。各向异性过滤级别可以通过调整 ProjectSettings.rendering/textures/default_filters/anisotropic_filtering_level 来改变。

注意：该纹理过滤在 2D 项目中很少有用。TEXTURE_FILTER_NEAREST_WITH_MIPMAPS 在这种情况下通常更合适。

● TEXTURE_FILTER_LINEAR_WITH_MIPMAPS_ANISOTROPIC = 6
纹理过滤在最近的 4 个像素之间进行混合，并基于表面与相机视图之间的角度在 2 个多级渐远纹理之间进行混合（或者如果 ProjectSettings.rendering/textures/default_filters/use_nearest_mipmap_filter 为 true，则使用最近的多级渐远纹理）。这使得纹理从近处看起来平滑，从远处看起来也平滑。各向异性过滤提高了几乎与相机位于一条线的表面上的纹理质量，但速度稍慢。各向异性过滤级别可以通过调整 ProjectSettings.rendering/textures/default_filters/anisotropic_filtering_level 来改变。

注意：该纹理过滤在 2D 项目中很少有用。TEXTURE_FILTER_LINEAR_WITH_MIPMAPS 在这种情况下通常更合适。

● TEXTURE_FILTER_MAX = 7
代表 TextureFilter 枚举的大小。

enum  TextureRepeat:

● TEXTURE_REPEAT_PARENT_NODE = 0
该 CanvasItem 将从其父级继承过滤器。
● TEXTURE_REPEAT_DISABLED = 1
纹理不会重复。
● TEXTURE_REPEAT_ENABLED = 2
纹理将正常重复。
● TEXTURE_REPEAT_MIRROR = 3
纹理将以 2x2 平铺模式重复，其中偶数位置的元素会被镜像。
● TEXTURE_REPEAT_MAX = 4
代表 TextureRepeat 枚举的大小。

enum  ClipChildrenMode:

● CLIP_CHILDREN_DISABLED = 0
子级绘制在父级之上，不会被裁剪。
● CLIP_CHILDREN_ONLY = 1
父级仅用于裁剪目的。子级被裁剪到父级的可见区域，不绘制父级。
● CLIP_CHILDREN_AND_DRAW = 2
父级用于裁剪子级，但在将子级剪裁到其可见区域之前，父级也像往常一样绘制在子级下方。
● CLIP_CHILDREN_MAX = 3
代表 ClipChildrenMode 枚举的大小。


常量
● NOTIFICATION_TRANSFORM_CHANGED = 2000
该 CanvasItem 的全局变换已更改。只有在通过 set_notify_transform() 启用时，才会收到这个通知。
● NOTIFICATION_LOCAL_TRANSFORM_CHANGED = 35
该 CanvasItem 的局部变换已更改。只有在通过 set_notify_local_transform() 启用时，才会收到这个通知。
● NOTIFICATION_DRAW = 30
要求绘制该 CanvasItem（见 _draw()）。
● NOTIFICATION_VISIBILITY_CHANGED = 31
该 CanvasItem 的可见性已更改。
● NOTIFICATION_ENTER_CANVAS = 32
该 CanvasItem 已进入画布。
● NOTIFICATION_EXIT_CANVAS = 33
该 CanvasItem 已退出画布。
● NOTIFICATION_WORLD_2D_CHANGED = 36
该 CanvasItem 的活动 World2D 已更改。

属性说明

● ClipChildrenMode clip_children [默认： 0]set_clip_children_mode(值) setterget_clip_children_mode() getter

允许当前节点裁剪子节点，本质上是充当遮罩。


● int light_mask [默认： 1]set_light_mask(值) setterget_light_mask() getter

该 CanvasItem 的渲染层，用于响应 Light2D 节点。


● Material materialset_material(值) setterget_material() getter

应用于这个 CanvasItem 的材质。


● Color modulate [默认： Color(1, 1, 1, 1)]set_modulate(值) setterget_modulate() getter

应用于这个 CanvasItem 的颜色。这个属性会影响子级 CanvasItem，与只会影响节点自身的 self_modulate 不同。


● Color self_modulate [默认： Color(1, 1, 1, 1)]set_self_modulate(值) setterget_self_modulate() getter

应用于这个 CanvasItem 的颜色。这个属性不会影响子级 CanvasItem，与会同时影响节点自身和子级的 modulate 不同。

注意：内部子节点（例如 ColorPicker 中的滑动条、TabContainer 中的选项卡栏）也不受这个属性的影响（见 Node.get_child() 等类似方法的 include_internal 参数）。


● bool show_behind_parent [默认： false]set_draw_behind_parent(值) setteris_draw_behind_parent_enabled() getter

如果为 true，则对象在其父对象后面绘制。


● TextureFilter texture_filter [默认： 0]set_texture_filter(值) setterget_texture_filter() getter

在该 CanvasItem 上使用的纹理过滤模式。


● TextureRepeat texture_repeat [默认： 0]set_texture_repeat(值) setterget_texture_repeat() getter

在该 CanvasItem 上使用的纹理重复模式。


● bool top_level [默认： false]set_as_top_level(值) setteris_set_as_top_level() getter

如果为 true，则该 CanvasItem 不会继承父级 CanvasItem 的变换。它的绘制顺序也会发生改变，会在其他没有将 top_level 设置为 true 的 CanvasItem 之上绘制。效果和把该 CanvasItem 作为裸 Node 的子级一样。


● bool use_parent_material [默认： false]set_use_parent_material(值) setterget_use_parent_material() getter

如果为 true，则将父级 CanvasItem 的 material 属性用作此项的材质。


● int visibility_layer [默认： 1]set_visibility_layer(值) setterget_visibility_layer() getter

Viewport 节点渲染该 CanvasItem 时所使用的渲染层。只有 CanvasItem 及其所有父级均与 Viewport 的画布剔除遮罩有交集，该 Viewport 才会渲染此 CanvasItem。


● bool visible [默认： true]set_visible(值) setteris_visible() getter

如果为 true，这个 CanvasItem 被绘制。只有当它的所有父节点也可见时，该节点才是可见的（换句话说，is_visible_in_tree() 必须返回 true）。

注意：对于继承了 Popup 的控件，使其可见的正确方法是调用多个 popup*() 函数之一。


● bool y_sort_enabled [默认： false]set_y_sort_enabled(值) setteris_y_sort_enabled() getter

如果为 true，则会在绘制 Y 位置最低的子节点之后再绘制 Y 位置较高的子节点。如果为 false，则禁用 Y 排序。Y 排序仅影响继承自 CanvasItem 的子节点。

可以将 Y 排序的节点进行嵌套。子级 Y 排序的节点，会与父级在同一空间中进行 Y 排序。此功能可以让你在不更改场景树的情况下，更好地组织场景，或者将场景分为多个场景。


● bool z_as_relative [默认： true]set_z_as_relative(值) setteris_z_relative() getter

如果为 true，节点的 Z 索引是相对于它的父节点的 Z 索引而言的。如果这个节点的 Z 索引是 2，它的父节点的实际 Z 索引是 3，那么这个节点的实际 Z 索引将是 2 + 3 = 5。


● int z_index [默认： 0]set_z_index(值) setterget_z_index() getter

Z 索引。控制节点的渲染顺序。具有较高 Z 索引的节点将显示在其他节点的前面。必须在 RenderingServer.CANVAS_ITEM_Z_MIN 和 RenderingServer.CANVAS_ITEM_Z_MAX之间（包含）。

注意：改变 Control 的 Z 索引只影响绘图顺序，不影响处理输入事件的顺序。可用于实现某些 UI 动画，例如对处于悬停状态的菜单项进行缩放，此时会与其他内容重叠。


方法说明

● void _draw() virtual

当 CanvasItem 被请求重绘时调用（手动调用或者引擎调用 queue_redraw() 之后）。

对应于 Object._notification() 中的 NOTIFICATION_DRAW 通知。


● void draw_animation_slice(animation_length: float, slice_begin: float, slice_end: float, offset: float = 0.0)

后续的绘制命令将被忽略，除非它们位于指定的动画切片内。这是实现在背景上循环而不是不断重绘的动画的更快方法。


● void draw_arc(center: Vector2, radius: float, start_angle: float, end_angle: float, point_count: int, color: Color, width: float = -1.0, antialiased: bool = false)

使用一个 uniform color 和 width 以及可选的抗锯齿（仅支持正 width ），在给定的角度之间绘制一条未填充的弧线。point_count 的值越大，该曲线越平滑。另见 draw_circle()。

如果 width 为负，则它将被忽略，并使用 RenderingServer.PRIMITIVE_LINE_STRIP 绘制该弧线。这意味着当缩放 CanvasItem 时，弧线将保持细长。如果不需要此行为，请传递一个正的 width，如 1.0。

如果 start_angle < end_angle ，则圆弧是从 start_angle 朝向 end_angle 的值绘制的，即是顺时针方向；否则为逆时针方向。以相反的顺序传递相同的角度，将产生相同的弧线。如果 start_angle 和 end_angle 的差的绝对值大于 @GDScript.TAU 弧度，则绘制一个完整的圆弧（即弧线不会与自身重叠）。


● void draw_char(font: Font, pos: Vector2, char: String, font_size: int = 16, modulate: Color = Color(1, 1, 1, 1)) const

使用自定义字体绘制字符串的第一个字符。


● void draw_char_outline(font: Font, pos: Vector2, char: String, font_size: int = 16, size: int = -1, modulate: Color = Color(1, 1, 1, 1)) const

使用自定义字体绘制字符串中第一个字符的轮廓。


● void draw_circle(position: Vector2, radius: float, color: Color)

绘制彩色的实心圆。另见 draw_arc()、draw_polyline() 和 draw_polygon()。


● void draw_colored_polygon(points: PackedVector2Array, color: Color, uvs: PackedVector2Array = PackedVector2Array(), texture: Texture2D = null)

绘制一个由任意数量的点组成的彩色多边形，凸形或凹形。与 draw_polygon() 不同，必须为整个多边形指定一个单一颜色。


● void draw_dashed_line(from: Vector2, to: Vector2, color: Color, width: float = -1.0, dash: float = 2.0, aligned: bool = true)

使用给定的颜色和宽度，从一个 2D 点到另一个点绘制一条虚线。另见 draw_multiline() 和 draw_polyline()。

如果 width 为负，则将绘制一个两点图元而不是一个四点图元。这意味着当缩放 CanvasItem 时，线条部分将保持细长。如果不需要此行为，请传递一个正的 width，如 1.0。


● void draw_end_animation()

通过 draw_animation_slice() 提交所有动画切片后，该函数可以被用来将绘制恢复到其默认状态（所有后续绘制命令都将可见）。如果不关心这个特定用例，则不需要在提交切片后使用该函数。


● void draw_lcd_texture_rect_region(texture: Texture2D, rect: Rect2, src_rect: Rect2, modulate: Color = Color(1, 1, 1, 1))

在给定的位置绘制一个带有 LCD 子像素抗锯齿的字体纹理的矩形区域，可以选择用一种颜色来调制。

纹理是通过以下混合操作绘制的，CanvasItemMaterial 的混合模式被忽略：

dst.r = texture.r * modulate.r * modulate.a + dst.r * (1.0 - texture.r * modulate.a);
dst.g = texture.g * modulate.g * modulate.a + dst.g * (1.0 - texture.g * modulate.a);
dst.b = texture.b * modulate.b * modulate.a + dst.b * (1.0 - texture.b * modulate.a);
dst.a = modulate.a + dst.a * (1.0 - modulate.a);


● void draw_line(from: Vector2, to: Vector2, color: Color, width: float = -1.0, antialiased: bool = false)

使用给定的颜色和宽度，从一个 2D 点到另一个点绘制一条直线。它可以选择抗锯齿。另请参阅 draw_multiline() 和 draw_polyline()。

如果 width 为负，则将绘制一个两点图元而不是一个四点图元。这意味着当缩放 CanvasItem 时，线条将保持细长。如果不需要此行为，请传递一个正的 width，如 1.0。


● void draw_mesh(mesh: Mesh, texture: Texture2D, transform: Transform2D = Transform2D(1, 0, 0, 1, 0, 0), modulate: Color = Color(1, 1, 1, 1))

使用所提供的纹理以 2D 方式绘制一个 Mesh。相关文档请参阅 MeshInstance2D。


● void draw_msdf_texture_rect_region(texture: Texture2D, rect: Rect2, src_rect: Rect2, modulate: Color = Color(1, 1, 1, 1), outline: float = 0.0, pixel_range: float = 4.0, scale: float = 1.0)

在给定位置，绘制一条多通道有符号距离场纹理的纹理矩形区域，可以选择用一种颜色来调制。有关 MSDF 字体渲染的更多信息和注意事项，请参阅 FontFile.multichannel_signed_distance_field。

如果 outline 为正，则区域中像素的每个 Alpha 通道值都被设置为 outline 半径内真实距离的最大值。

pixel_range 的值应该与距离场纹理生成期间使用的值相同。


● void draw_multiline(points: PackedVector2Array, color: Color, width: float = -1.0)

使用一致的宽度 width 和颜色 color 绘制多条断开的线段。points 数组中相邻的两个点定义一条线段，即第 i 条线段由端点 points[2 * i] 和 points[2 * i + 1] 组成。绘制大量线段时，这种方法比使用 draw_line() 一条条画要快。要绘制相连的线段，请改用 draw_polyline()。

如果 width 为负数，则会绘制由两个点组成的图元，不使用四个点组成的图元。此时如果 CanvasItem 发生缩放，则线段仍然会很细。如果不想要这样的行为，请传入 1.0 等正数 width。


● void draw_multiline_colors(points: PackedVector2Array, colors: PackedColorArray, width: float = -1.0)

使用一致的宽度 width 分段颜色绘制多条断开的线段。points 数组中相邻的两个点定义一条线段，即第 i 条线段由端点 points[2 * i] 和 points[2 * i + 1] 组成，使用的颜色为 colors[i]。绘制大量线段时，这种方法比使用 draw_line() 一条条画要快。要绘制相连的线段，请改用 draw_polyline_colors()。

如果 width 为负数，则会绘制由两个点组成的图元，不使用四个点组成的图元。此时如果 CanvasItem 发生缩放，则线段仍然会很细。如果不想要这样的行为，请传入 1.0 等正数 width。


● void draw_multiline_string(font: Font, pos: Vector2, text: String, alignment: HorizontalAlignment = 0, width: float = -1, font_size: int = 16, max_lines: int = -1, modulate: Color = Color(1, 1, 1, 1), brk_flags: BitField[TextServer.LineBreakFlag] = 3, justification_flags: BitField[TextServer.JustificationFlag] = 3, direction: TextServer.Direction = 0, orientation: TextServer.Orientation = 0) const

将 text 分成几行，并在 pos（左上角）处使用指定的 font 绘制文本。该文本的颜色将乘以 modulate。如果 width 大于等于 0，则当该文本超过指定宽度时将被裁剪。


● void draw_multiline_string_outline(font: Font, pos: Vector2, text: String, alignment: HorizontalAlignment = 0, width: float = -1, font_size: int = 16, max_lines: int = -1, size: int = 1, modulate: Color = Color(1, 1, 1, 1), brk_flags: BitField[TextServer.LineBreakFlag] = 3, justification_flags: BitField[TextServer.JustificationFlag] = 3, direction: TextServer.Direction = 0, orientation: TextServer.Orientation = 0) const

将 text 分成几行，并在 pos（左上角）处使用指定的 font 绘制文本轮廓。该文本的颜色将乘以 modulate。如果 width 大于等于 0，则当该文本超过指定宽度时将被裁剪。


● void draw_multimesh(multimesh: MultiMesh, texture: Texture2D)

用所提供的纹理以 2D 方式绘制一个 MultiMesh。相关文档请参考 MultiMeshInstance2D。


● void draw_polygon(points: PackedVector2Array, colors: PackedColorArray, uvs: PackedVector2Array = PackedVector2Array(), texture: Texture2D = null)

绘制一个由任意数量的点构成的实心多边形，凸形或凹形。与 draw_colored_polygon() 不同，每个点的颜色都可以单独改变。另见 draw_polyline() 和 draw_polyline_colors()。如果你需要更大的自由度（例如能够使用骨骼），请改用 RenderingServer.canvas_item_add_triangle_array()。


● void draw_polyline(points: PackedVector2Array, color: Color, width: float = -1.0, antialiased: bool = false)

使用一致的 color 和 width 以及可选的抗锯齿（仅支持正 width ），绘制相互连接的线段。绘制大量线条时，这比使用单独的 draw_line() 调用更快。要绘制不相连的的线段，请改用 draw_multiline()。另见 draw_polygon()。

如果 width 为负，则它将被忽略，并使用 RenderingServer.PRIMITIVE_LINE_STRIP 绘制该折线。这意味着当 CanvasItem 被缩放时，折线将保持为细线。如果不需要该行为，请传入一个正的 width，如 1.0。


● void draw_polyline_colors(points: PackedVector2Array, colors: PackedColorArray, width: float = -1.0, antialiased: bool = false)

绘制相连的线段，使用一致的宽度 width，按点指定颜色，还可以开启抗锯齿（仅支持正的 width）。将颜色与线段上的点匹配时，使用的是 points 和 colors 的索引，即每条线段填充的都是在两个端点之间颜色的渐变色。绘制大量线段时，这种方法比使用 draw_line() 一条条画要快。要绘制不相连的线段，请改用 draw_multiline_colors()。另见 draw_polygon()。

如果 width 为负，则它将被忽略，并使用 RenderingServer.PRIMITIVE_LINE_STRIP 绘制该折线。这意味着当 CanvasItem 被缩放时，折线将保持为细线。如果不需要该行为，请传入一个正的 width，如 1.0。


● void draw_primitive(points: PackedVector2Array, colors: PackedColorArray, uvs: PackedVector2Array, texture: Texture2D = null)

绘制自定义图元。1 个点的是个点，2 个点的是线段，3 个点的是三角形，4 个点的是四边形。如果没有指定点或者指定了超过 4 个点，则不会绘制任何东西，只会输出错误消息。另请参阅 draw_line()、draw_polyline()、draw_polygon()、draw_rect()。


● void draw_rect(rect: Rect2, color: Color, filled: bool = true, width: float = -1.0)

绘制一个矩形。如果 filled 为 true，则矩形将使用指定的 color 填充。如果 filled 为 false，则矩形将被绘制为具有指定的 color 和 width 的笔划。另见 draw_texture_rect()。

如果 width 为负，则将绘制一个两点图元而不是一个四点图元。这意味着当缩放 CanvasItem 时，线条将保持细长。如果不需要此行为，请传递一个正的 width，如 1.0。

注意：width 只有在 filled 为 false 时才有效。

注意：使用负 width 绘制的未填充矩形可能不会完美显示。例如，由于线条的重叠，角可能会缺失或变亮（对于半透明的 color）。


● void draw_set_transform(position: Vector2, rotation: float = 0.0, scale: Vector2 = Vector2(1, 1))

使用分量设置用于绘图的自定义变换。后续的绘制都会使用这个变换。

注意：FontFile.oversampling 不会考虑 scale。这意味着将位图字体及栅格化（非 MSDF）动态字体放大/缩小会产生模糊或像素化的结果。要让文本无论如何缩放都保持清晰，可以启用 MSDF 字体渲染，方法是启用 ProjectSettings.gui/theme/default_font_multichannel_signed_distance_field（仅应用于默认项目字体），或者启用自定义 DynamicFont 的多通道带符号距离场导入选项。对于系统字体，可以在检查器中启用 SystemFont.multichannel_signed_distance_field。


● void draw_set_transform_matrix(xform: Transform2D)

设置通过矩阵绘制时的自定义变换。此后绘制的任何东西都将被它变换。


● void draw_string(font: Font, pos: Vector2, text: String, alignment: HorizontalAlignment = 0, width: float = -1, font_size: int = 16, modulate: Color = Color(1, 1, 1, 1), justification_flags: BitField[TextServer.JustificationFlag] = 3, direction: TextServer.Direction = 0, orientation: TextServer.Orientation = 0) const

使用指定的 font 在 pos（使用的字体的基线的左下角）处绘制 text。该文本的颜色将乘以 modulate。如果 width 大于等于 0，则文本超过指定宽度将被裁剪。

使用项目默认字体的例子：

==如果在不断重绘的脚本中使用此方法，==

==则将 default_font` 声明移动到在 `_ready()` 中赋值的成员变量中==

==这样 Control 只创建一次。==

var default_font = ThemeDB.fallback_font
var default_font_size = ThemeDB.fallback_font_size
draw_string(default_font, Vector2(64, 64), "Hello world", HORIZONTAL_ALIGNMENT_LEFT, -1, default_font_size)

另请参阅 Font.draw_string()。


● void draw_string_outline(font: Font, pos: Vector2, text: String, alignment: HorizontalAlignment = 0, width: float = -1, font_size: int = 16, size: int = 1, modulate: Color = Color(1, 1, 1, 1), justification_flags: BitField[TextServer.JustificationFlag] = 3, direction: TextServer.Direction = 0, orientation: TextServer.Orientation = 0) const

在 pos（左下角使用字体的基线）处使用指定的 font 绘制 text 轮廓。该文本的颜色将乘以 modulate。如果 width 大于等于 0，则当文本超过指定宽度时将被裁剪。


● void draw_style_box(style_box: StyleBox, rect: Rect2)

绘制一个样式矩形。


● void draw_texture(texture: Texture2D, position: Vector2, modulate: Color = Color(1, 1, 1, 1))

在给定的位置绘制纹理。


● void draw_texture_rect(texture: Texture2D, rect: Rect2, tile: bool, modulate: Color = Color(1, 1, 1, 1), transpose: bool = false)

在给定位置绘制一个带纹理的矩形，可以选择用颜色调制。如果 transpose 为 true，则纹理将交换其 X 和 Y 坐标。另见 draw_rect() 和 draw_texture_rect_region()。


● void draw_texture_rect_region(texture: Texture2D, rect: Rect2, src_rect: Rect2, modulate: Color = Color(1, 1, 1, 1), transpose: bool = false, clip_uv: bool = true)

在给定的位置绘制具有纹理的矩形，可以指定所使用的纹理区域（由 src_rect 指定），可选择用颜色调制。如果 transpose 为 true，则纹理将交换其 X 和 Y 坐标。另见 draw_texture_rect()。


● void force_update_transform()

强制更新变换。由于性能原因，物理中的变换改变不是即时的。变换是在累积后再设置。如果你在进行物理操作时需要最新的变换，请使用此功能。


● RID get_canvas() const

返回此项目所在的 World2D 画布的 RID。


● RID get_canvas_item() const

返回 RenderingServer 对该项目使用的画布项目 RID。


● Transform2D get_canvas_transform() const

返回从该项目所在的画布坐标系到 Viewport 坐标系的变换。


● Vector2 get_global_mouse_position() const

返回该 CanvasItem 所在的 CanvasLayer 中鼠标的位置，使用该 CanvasLayer 的坐标系。

注意：要得到屏幕空间的坐标（例如使用非嵌入式 Popup 时），你可以使用 DisplayServer.mouse_get_position()。


● Transform2D get_global_transform() const

返回该项目的全局变换矩阵，即到最顶层的 CanvasItem 节点的综合变换。最顶层的项目是一个 CanvasItem，它要么没有父级，要么有非 CanvasItem 父级，或者要么它启用了 top_level。


● Transform2D get_global_transform_with_canvas() const

返回从该 CanvasItem 的局部坐标系到 Viewport 坐标系的变换。


● Vector2 get_local_mouse_position() const

返回该 CanvasItem 中鼠标的位置，使用该 CanvasItem 的局部坐标系。


● Transform2D get_screen_transform() const

返回该 CanvasItem 在全局屏幕坐标中的变换（即考虑窗口位置）。主要用于编辑器插件。

如果窗口是嵌入的，则等于 get_global_transform()（参见 Viewport.gui_embed_subwindows）。


● Transform2D get_transform() const

返回此项目的变换矩阵。


● Rect2 get_viewport_rect() const

以 Rect2 形式返回视口的边界。


● Transform2D get_viewport_transform() const

返回从该项目所在的画布坐标系到 Viewport 嵌入坐标系的变换。


● bool get_visibility_layer_bit(layer: int) const

返回渲染可见层上的某个比特位。


● World2D get_world_2d() const

返回此物品所在的 World2D。


● void hide()

如果该 CanvasItem 目前是可见的，则将其隐藏。相当于将 visible 设为 false。


● bool is_local_transform_notification_enabled() const

如果将局部变换通知传达给子级，则返回 true。


● bool is_transform_notification_enabled() const

如果将全局变换通知传达给子级，则返回 true。


● bool is_visible_in_tree() const

如果该节点位于 SceneTree 中，并且其 visible 属性为 true，并且其所有上层节点也均可见，则返回 true。如果任何上层节点被隐藏，则该节点在场景树中将不可见，因此也不会进行绘制（见 _draw()）。


● Vector2 make_canvas_position_local(screen_point: Vector2) const

将 screen_point 指定为该节点的新局部变换。


● InputEvent make_input_local(event: InputEvent) const

event 的输入发出的变换将在局部空间而不是全局空间中应用。


● void move_to_front()

移动该节点以显示在其同级节点之上。

在内部，该节点被移动到父节点的子节点列表的底部。该方法对没有父节点的节点没有影响。


● void queue_redraw()

将该 CanvasItem 加入重绘队列。空闲时，如果 CanvasItem 可见，则会发送 NOTIFICATION_DRAW 并调用 _draw()。即便多次调用这个方法，每帧也都只会发生一次绘制。


● void set_notify_local_transform(enable: bool)

如果 enable 为 true，则该节点将在其局部变换发生改变时收到 NOTIFICATION_LOCAL_TRANSFORM_CHANGED。


● void set_notify_transform(enable: bool)

如果 enable 为 true，那么这个节点会在其全局变换发生改变时接收到 NOTIFICATION_TRANSFORM_CHANGED。


● void set_visibility_layer_bit(layer: int, enabled: bool)

设置或清除渲染可见层上的单个位。这简化了对该 CanvasItem 的可见层的编辑。


● void show()

如果该 CanvasItem 目前是隐藏的，则将其显示。相当于将 visible 设为 true。对于继承自 Popup 的控件，让它们可见的正确做法是换成调用各种 popup*() 函数的其中之一。

---

---



## 类：   Node2D

继承：   CanvasItem <   Node <   Object
派生：   AnimatedSprite2D ,   AudioListener2D ,   AudioStreamPlayer2D ,   BackBufferCopy ,   Bone2D ,   CPUParticles2D ,   Camera2D ,   CanvasGroup ,   CanvasModulate ,   CollisionObject2D ,   CollisionPolygon2D ,   CollisionShape2D ,   GPUParticles2D ,   Joint2D ,   Light2D ,   LightOccluder2D ,   Line2D ,   Marker2D ,   MeshInstance2D ,   MultiMeshInstance2D ,   NavigationLink2D ,   NavigationObstacle2D ,   NavigationRegion2D ,   ParallaxLayer ,   Path2D ,   PathFollow2D ,   Polygon2D ,   RayCast2D ,   RemoteTransform2D ,   ShapeCast2D ,   Skeleton2D ,   Sprite2D ,   TileMap ,   TouchScreenButton ,   VisibleOnScreenNotifier2D


2D 游戏对象，被所有 2D 相关的节点继承。具有位置、旋转、缩放和 Z 索引。


描述

2D 游戏对象，具有变换（位置、旋转、缩放）。所有的 2D 节点，包括物理对象和精灵，都继承自 Node2D。使用 Node2D 作为父节点来移动、缩放和旋转 2D 项目中的子节点。还可以控制节点的渲染顺序。


在线教程
2D 中的自定义绘图
所有 2D 演示


属性
Vector2global_positionfloatglobal_rotationfloatglobal_rotation_degreesVector2global_scalefloatglobal_skewTransform2Dglobal_transformVector2position [默认： Vector2(0, 0)]floatrotation [默认： 0.0]floatrotation_degreesVector2scale [默认： Vector2(1, 1)]floatskew [默认： 0.0]Transform2Dtransform

方法
voidapply_scale(ratio: Vector2)floatget_angle_to(point: Vector2) constTransform2Dget_relative_transform_to_parent(parent: Node) constvoidglobal_translate(offset: Vector2)voidlook_at(point: Vector2)voidmove_local_x(delta: float, scaled: bool = false)voidmove_local_y(delta: float, scaled: bool = false)voidrotate(radians: float)Vector2to_global(local_point: Vector2) constVector2to_local(global_point: Vector2) constvoidtranslate(offset: Vector2)

属性说明

● Vector2 global_positionset_global_position(值) setterget_global_position() getter

全局位置。


● float global_rotationset_global_rotation(值) setterget_global_rotation() getter

全局旋转，单位为弧度。


● float global_rotation_degreesset_global_rotation_degrees(值) setterget_global_rotation_degrees() getter

辅助属性，用于按度数访问 global_rotation 而不是弧度数。


● Vector2 global_scaleset_global_scale(值) setterget_global_scale() getter

全局缩放。


● float global_skewset_global_skew(值) setterget_global_skew() getter

全局偏斜，单位为弧度。


● Transform2D global_transformset_global_transform(值) setterget_global_transform() getter

全局 Transform2D。


● Vector2 position [默认： Vector2(0, 0)]set_position(值) setterget_position() getter

位置，相对于父节点。


● float rotation [默认： 0.0]set_rotation(值) setterget_rotation() getter

旋转，单位为弧度，相对于该节点的父节点。

注意：这个属性在检查器中是以度数编辑的。如果你想在脚本中使用度数，请使用 rotation_degrees。


● float rotation_degreesset_rotation_degrees(值) setterget_rotation_degrees() getter

辅助属性，用于按度数访问 rotation 而不是弧度数。


● Vector2 scale [默认： Vector2(1, 1)]set_scale(值) setterget_scale() getter

该节点的缩放。未缩放值：(1, 1)。

注意：2D 中，变换矩阵是无法分解出负数的 X 缩放的。由于 Godot 中使用变换矩阵来表示缩放，X 轴上的负数缩放在分解后会变为 Y 轴的负数缩放和一次 180 度的旋转。


● float skew [默认： 0.0]set_skew(值) setterget_skew() getter

使该节点发生倾斜。

注意：偏斜仅适用于 X 轴。


● Transform2D transformset_transform(值) setterget_transform() getter

局部 Transform2D。


方法说明

● void apply_scale(ratio: Vector2)

将当前缩放乘以比例向量 ratio。


● float get_angle_to(point: Vector2) const

返回该节点和 point 之间的夹角，单位为弧度。

返回夹角的示意图。


● Transform2D get_relative_transform_to_parent(parent: Node) const

返回相对于此节点的父节点的 Transform2D。


● void global_translate(offset: Vector2)

将偏移向量 offset 添加到该节点的全局位置。


● void look_at(point: Vector2)

旋转该节点，使其指向 point，该点应使用全局坐标。


● void move_local_x(delta: float, scaled: bool = false)

基于 Node._process() 的 delta，在节点的 X 轴上应用局部平移。如果 scaled 为 false，则对移动进行归一化。


● void move_local_y(delta: float, scaled: bool = false)

基于 Node._process() 的 delta，在节点的 Y 轴上应用局部平移。如果 scaled 为 false，则对移动进行归一化。


● void rotate(radians: float)

从节点的当前旋转开始，以弧度为单位，对节点进行旋转。


● Vector2 to_global(local_point: Vector2) const

将提供的本地位置转换为全局坐标空间的位置。例如，对子节点的位置应用这个方法将正确地把它们的位置转换到全局坐标空间，但对节点自己的位置应用这个方法将得到一个不正确的结果，因为它将把节点自己的变换纳入它的全局位置。


● Vector2 to_local(global_point: Vector2) const

将提供的全局位置转换为本地坐标空间的位置。例如，它适合于确定子节点的位置，但不适合于确定其自身相对于父节点的位置。


● void translate(offset: Vector2)

在局部坐标系中，将该节点按给定的偏移量 offset 进行平移。

---

---

## 类：   Sprite2D

继承：   Node2D <   CanvasItem <   Node <   Object


通用精灵节点。


描述

显示 2D 纹理的节点。显示的纹理可以是较大图集纹理中的某个区域，也可以是精灵表动画中的某一帧。


在线教程
实例化演示


属性
boolcentered [默认： true]boolflip_h [默认： false]boolflip_v [默认： false]intframe [默认： 0]Vector2iframe_coords [默认： Vector2i(0, 0)]inthframes [默认： 1]Vector2offset [默认： Vector2(0, 0)]boolregion_enabled [默认： false]boolregion_filter_clip_enabled [默认： false]Rect2region_rect [默认： Rect2(0, 0, 0, 0)]Texture2Dtextureintvframes [默认： 1]

方法
Rect2get_rect() constboolis_pixel_opaque(pos: Vector2) const

信号

● frame_changed()当 frame 更改时发出。

● texture_changed()当 texture 更改时发出。


属性说明

● bool centered [默认： true]set_centered(值) setteris_centered() getter

如果为 true，纹理居中。


● bool flip_h [默认： false]set_flip_h(值) setteris_flipped_h() getter

如果为 true，纹理将被水平翻转。


● bool flip_v [默认： false]set_flip_v(值) setteris_flipped_v() getter

如果为 true，纹理将被垂直翻转。


● int frame [默认： 0]set_frame(值) setterget_frame() getter

当前显示的精灵表中的帧。vframes 或 hframes 必须大于 1。


● Vector2i frame_coords [默认： Vector2i(0, 0)]set_frame_coords(值) setterget_frame_coords() getter

显示的帧在精灵表中的坐标。这是 frame 属性的别名。vframes 或 hframes 必须大于 1。


● int hframes [默认： 1]set_hframes(值) setterget_hframes() getter

精灵表中的列数。


● Vector2 offset [默认： Vector2(0, 0)]set_offset(值) setterget_offset() getter

纹理的绘图偏移量。


● bool region_enabled [默认： false]set_region_enabled(值) setteris_region_enabled() getter

如果为 true，则从较大的图集纹理中剪切纹理。见 region_rect。


● bool region_filter_clip_enabled [默认： false]set_region_filter_clip_enabled(值) setteris_region_filter_clip_enabled() getter

如果为 true，则最外侧的像素会变得模糊。region_enabled 必须为 true。


● Rect2 region_rect [默认： Rect2(0, 0, 0, 0)]set_region_rect(值) setterget_region_rect() getter

要显示的图集纹理区域。region_enabled 必须是 true。


● Texture2D textureset_texture(值) setterget_texture() getter

要绘制的 Texture2D 对象。


● int vframes [默认： 1]set_vframes(值) setterget_vframes() getter

精灵表中的行数。


方法说明

● Rect2 get_rect() const

返回代表该 Sprite2D 边界的 Rect2，使用局部坐标。可用于检测该 Sprite2D 是否被点击。

示例：

func _input(event):
    if event is InputEventMouseButton and event.pressed and event.button_index == MOUSE_BUTTON_LEFT:
        if get_rect().has_point(to_local(event.position)):
            print("我点！")


● bool is_pixel_opaque(pos: Vector2) const

如果给定位置的像素不透明，则返回 true，其他情况下返回 false。

注意：如果精灵的纹理为 null 或者给定的位置无效，它也会返回 false。

---

---

## 类：   CollisionObject2D

继承：   Node2D <   CanvasItem <   Node <   Object
派生：   Area2D ,   PhysicsBody2D


2D 物理对象的抽象基类。


描述

2D 物理对象的抽象基类。CollisionObject2D 能够容纳任意数量的 Shape2D 用作碰撞形状。每个形状必须分配给一个形状所有者。形状所有者不是节点，也不会出现在编辑器中，但可以通过代码使用 shape_owner_* 方法访问。

注意：仅支持相同画布中不同对象的碰撞（Viewport 画布或 CanvasLayer）。不同画布中的对象之间的碰撞行为是未定义的。


属性
intcollision_layer [默认： 1]intcollision_mask [默认： 1]floatcollision_priority [默认： 1.0]DisableModedisable_mode [默认： 0]boolinput_pickable [默认： true]

方法
void_input_event(viewport: Viewport, event: InputEvent, shape_idx: int) virtualvoid_mouse_enter() virtualvoid_mouse_exit() virtualvoid_mouse_shape_enter(shape_idx: int) virtualvoid_mouse_shape_exit(shape_idx: int) virtualintcreate_shape_owner(owner: Object)boolget_collision_layer_value(layer_number: int) constboolget_collision_mask_value(layer_number: int) constRIDget_rid() constfloatget_shape_owner_one_way_collision_margin(owner_id: int) constPackedInt32Arrayget_shape_owners()boolis_shape_owner_disabled(owner_id: int) constboolis_shape_owner_one_way_collision_enabled(owner_id: int) constvoidremove_shape_owner(owner_id: int)voidset_collision_layer_value(layer_number: int, value: bool)voidset_collision_mask_value(layer_number: int, value: bool)intshape_find_owner(shape_index: int) constvoidshape_owner_add_shape(owner_id: int, shape: Shape2D)voidshape_owner_clear_shapes(owner_id: int)Objectshape_owner_get_owner(owner_id: int) constShape2Dshape_owner_get_shape(owner_id: int, shape_id: int) constintshape_owner_get_shape_count(owner_id: int) constintshape_owner_get_shape_index(owner_id: int, shape_id: int) constTransform2Dshape_owner_get_transform(owner_id: int) constvoidshape_owner_remove_shape(owner_id: int, shape_id: int)voidshape_owner_set_disabled(owner_id: int, disabled: bool)voidshape_owner_set_one_way_collision(owner_id: int, enable: bool)voidshape_owner_set_one_way_collision_margin(owner_id: int, margin: float)voidshape_owner_set_transform(owner_id: int, transform: Transform2D)

信号

● input_event(viewport: Node, event: InputEvent, shape_idx: int)当输入事件发生时发出。要求 input_pickable 为 true 并且至少设置了一个 collision_layer 位。详见 _input_event()。

● mouse_entered()当鼠标指针进入该对象的任何形状时发出。要求 input_pickable 为 true 并且至少设置了一个 collision_layer 位。请注意，在单个 CollisionObject2D 中的不同形状之间移动，不会导致发出该信号。

注意：由于缺少连续的碰撞检测，如果鼠标移动得足够快并且 CollisionObject2D 的区域很小，则该信号可能不会按预期的顺序发出。如果另一个 CollisionObject2D 与所讨论的 CollisionObject2D 重叠，则也可能不会发出该信号。

● mouse_exited()当鼠标指针离开该对象的所有形状时发出。要求 input_pickable 为 true 并且至少设置了一个 collision_layer 位。请注意，在单个 CollisionObject2D 中的不同形状之间移动，不会导致发出该信号。

注意：由于缺少连续的碰撞检测，如果鼠标移动得足够快并且 CollisionObject2D 的区域很小，则该信号可能不会按预期的顺序发出。如果另一个 CollisionObject2D 与所讨论的 CollisionObject2D 重叠，则也可能不会发出该信号。

● mouse_shape_entered(shape_idx: int)当鼠标指针进入该实体的任何形状或从一种形状移动到另一种形状时发出。shape_idx 是新进入的 Shape2D 的子索引。要求 input_pickable 为 true 并且至少设置一个 collision_layer 位。

● mouse_shape_exited(shape_idx: int)当鼠标指针离开该实体的任何形状时发出。shape_idx 是退出的 Shape2D 的子索引。要求 input_pickable 为 true 并且至少设置一个 collision_layer 位。


枚举
enum  DisableMode:

● DISABLE_MODE_REMOVE = 0
当 Node.process_mode 被设置为 Node.PROCESS_MODE_DISABLED 时，从物理仿真中移除，停止与此 CollisionObject2D 的所有物理交互。

当该 Node 再次被处理时，会自动重新加入到物理仿真中。

● DISABLE_MODE_MAKE_STATIC = 1
当 Node.process_mode 被设置为 Node.PROCESS_MODE_DISABLED 时，使物体进入静态模式。不影响 Area2D。处于静态模式的 PhysicsBody2D 不会受到力和其他物体的影响。

当该 Node 再次被处理时，会自动将 PhysicsBody2D 设置回其原始模式。

● DISABLE_MODE_KEEP_ACTIVE = 2
当 Node.process_mode 被设置为 Node.PROCESS_MODE_DISABLED 时，不影响物理仿真。


属性说明

● int collision_layer [默认： 1]set_collision_layer(值) setterget_collision_layer() getter

此 CollisionObject2D 所在的物理层。碰撞对象可以存在于 32 个不同层中的一个或多个中。另见 collision_mask。

注意：只有当对象 B 在对象 A 扫描的任何层中时，对象 A 才能检测到与对象 B 的接触。有关更多信息，请参阅文档中的《碰撞层与掩码》。


● int collision_mask [默认： 1]set_collision_mask(值) setterget_collision_mask() getter

此 CollisionObject2D 扫描的物理层。碰撞对象可以扫描 32 个不同层中的一个或多个。另见 collision_layer。

注意：只有当对象 B 在对象 A 扫描的任何层中时，对象 A 才能检测到与对象 B 的接触。有关更多信息，请参阅文档中的《碰撞层与掩码》。


● float collision_priority [默认： 1.0]set_collision_priority(值) setterget_collision_priority() getter

发生穿透时用于解决碰撞的优先级。优先级越高，对物体的穿透度就越低。例如，可以用来防止玩家突破关卡的边界。


● DisableMode disable_mode [默认： 0]set_disable_mode(值) setterget_disable_mode() getter

当 Node.process_mode 被设置为 Node.PROCESS_MODE_DISABLED 时，定义物理行为。有关不同模式的更多详细信息，请参阅 DisableMode。


● bool input_pickable [默认： true]set_pickable(值) setteris_pickable() getter

如果为 true，则该对象是可拾取的。可拾取的对象可以检测鼠标指针的进入/离开，鼠标位于其中时，就会报告输入事件。要求至少设置一个 collision_layer 位。


方法说明

● void _input_event(viewport: Viewport, event: InputEvent, shape_idx: int) virtual

接收未处理的 InputEvent。shape_idx 是被点击的 Shape2D 的子索引。连接到 input_event 即可轻松获取这些事件。

注意：_input_event() 要求 input_pickable 为 true，并且至少要设置一个 collision_layer 位。


● void _mouse_enter() virtual

当鼠标指针进入该实体的任何形状时调用。要求 input_pickable 为 true 并且至少设置了一个 collision_layer 位。请注意，在单个 CollisionObject2D 中的不同形状之间移动，不会导致该函数被调用。


● void _mouse_exit() virtual

当鼠标指针退出该实体的所有形状时调用。要求 input_pickable 为 true 并且至少设置了一个 collision_layer 位。请注意，在单个 CollisionObject2D 中的不同形状之间移动，不会导致该函数被调用。


● void _mouse_shape_enter(shape_idx: int) virtual

当鼠标指针进入该实体的任何形状或从一个形状移动到另一个形状时调用。shape_idx 是新进入的 Shape2D 的子索引。要求 input_pickable 为 true 并且要至少设置一个 collision_layer 位。


● void _mouse_shape_exit(shape_idx: int) virtual

当鼠标指针离开该实体的任何形状时调用。shape_idx 是退出的 Shape2D 的子索引。要求 input_pickable 为 true 并且至少要设置一个 collision_layer 位。


● int create_shape_owner(owner: Object)

为给定对象创建一个新的形状所有者。返回 owner_id的新所有者，供将来引用。


● bool get_collision_layer_value(layer_number: int) const

返回 collision_layer 中是否启用了指定的层，给定的 layer_number 应在 1 和 32 之间。


● bool get_collision_mask_value(layer_number: int) const

返回 collision_mask 中是否启用了指定的层，给定的 layer_number 应在 1 和 32 之间。


● RID get_rid() const

返回对象的 RID。


● float get_shape_owner_one_way_collision_margin(owner_id: int) const

返回由给定 owner_id 标识的形状所有者的 one_way_collision_margin。


● PackedInt32Array get_shape_owners()

返回一个 owner_id 标识符的 Array。你可以在其他使用 owner_id 作为参数的方法中使用这些 ID。


● bool is_shape_owner_disabled(owner_id: int) const

如果为 true，则禁用形状所有者及其形状。


● bool is_shape_owner_one_way_collision_enabled(owner_id: int) const

返回 true，如果源于这个 CollisionObject2D 的形状所有者的碰撞不会被报告给 CollisionObject2D。


● void remove_shape_owner(owner_id: int)

移除给定形状的所有者。


● void set_collision_layer_value(layer_number: int, value: bool)

根据 value，启用或禁用 collision_layer 中指定的层，给定的 layer_number 应在 1 和 32 之间。


● void set_collision_mask_value(layer_number: int, value: bool)

根据 value，启用或禁用 collision_mask 中指定的层，给定的 layer_number 应在 1 和 32 之间。


● int shape_find_owner(shape_index: int) const

返回指定形状的 owner_id。


● void shape_owner_add_shape(owner_id: int, shape: Shape2D)

给形状所有者添加一个 Shape2D。


● void shape_owner_clear_shapes(owner_id: int)

移除形状所有者的所有形状。


● Object shape_owner_get_owner(owner_id: int) const

返回给定形状所有者的父对象。


● Shape2D shape_owner_get_shape(owner_id: int, shape_id: int) const

从给定形状所有者返回具有给定 ID 的 Shape2D。


● int shape_owner_get_shape_count(owner_id: int) const

返回给定形状所有者包含的形状数量。


● int shape_owner_get_shape_index(owner_id: int, shape_id: int) const

从给定形状所有者返回具有给定 ID 的 Shape2D 的子索引。


● Transform2D shape_owner_get_transform(owner_id: int) const

返回形状所有者的 Transform2D。


● void shape_owner_remove_shape(owner_id: int, shape_id: int)

从给定的形状所有者中移除一个形状。


● void shape_owner_set_disabled(owner_id: int, disabled: bool)

如果为 true，则禁用给定的形状所有者。


● void shape_owner_set_one_way_collision(owner_id: int, enable: bool)

如果 enable 为 true，则源自该 CollisionObject2D 的形状所有者的碰撞将不会被报告为与 CollisionObject2D 发生碰撞。


● void shape_owner_set_one_way_collision_margin(owner_id: int, margin: float)

将由给定 owner_id 标识的形状所有者的 one_way_collision_margin 设置为 margin 像素。


● void shape_owner_set_transform(owner_id: int, transform: Transform2D)

设置给定形状所有者的 Transform2D。

---

---

## 类：   Area2D

继承：   CollisionObject2D <   Node2D <   CanvasItem <   Node <   Object


2D 空间中的一个区域，能够检测到其他 CollisionObject2D 的进入或退出。


描述

Area2D 是 2D 空间中的一个区域，由一个或多个 CollisionShape2D 或 CollisionPolygon2D 子节点定义，能够检测到其他 CollisionObject2D 进入或退出该区域，同时也会记录哪些碰撞对象尚未退出（即哪些对象与其存在重叠）。

这个节点也可以在局部修改或覆盖物理参数（重力、阻尼），将音频引导至自定义音频总线。


在线教程
使用 Area2D
2D Dodge The Creeps 演示
2D Pong 演示
2D 平台跳跃演示


属性
floatangular_damp [默认： 1.0]SpaceOverrideangular_damp_space_override [默认： 0]StringNameaudio_bus_name [默认： &"Master"]boolaudio_bus_override [默认： false]floatgravity [默认： 980.0]Vector2gravity_direction [默认： Vector2(0, 1)]boolgravity_point [默认： false]Vector2gravity_point_center [默认： Vector2(0, 1)]floatgravity_point_unit_distance [默认： 0.0]SpaceOverridegravity_space_override [默认： 0]floatlinear_damp [默认： 0.1]SpaceOverridelinear_damp_space_override [默认： 0]boolmonitorable [默认： true]boolmonitoring [默认： true]intpriority [默认： 0]

方法
Array[Area2D]get_overlapping_areas() constArray[Node2D]get_overlapping_bodies() constboolhas_overlapping_areas() constboolhas_overlapping_bodies() constbooloverlaps_area(area: Node) constbooloverlaps_body(body: Node) const

信号

● area_entered(area: Area2D)当接收的 area 进入此区域时发出。需要 monitoring 被设置为 true。

● area_exited(area: Area2D)当接收的 area 退出此区域时发出。需要 monitoring 被设置为 true。

● area_shape_entered(area_rid: RID, area: Area2D, area_shape_index: int, local_shape_index: int)当收到的 area 的 Shape2D 进入这个区域的形状时发出。要求 monitoring 被设置为 true 。

local_shape_index 和 area_shape_index 分别包含来自这个区域和另一个区域的交互形状的索引。area_rid 包含另一个区域的 RID。这些值可以与 PhysicsServer2D 一起使用。

从形状索引中获取 CollisionShape2D节点的例子：

var other_shape_owner = area.shape_find_owner( area_shape_index)
var other_shape_node = area.shape_owner_get_owner(other_shape_owner)

var local_shape_owner = shape_find_owner(local_shape_index)
var local_shape_node = shape_owner_get_owner(local_shape_owner)

● area_shape_exited(area_rid: RID, area: Area2D, area_shape_index: int, local_shape_index: int)当接收的 area 的 Shape2D 退出此区域的形状时发出。需要 monitoring 被设置为 true。

另见 area_shape_entered。

● body_entered(body: Node2D)当接收到的 body 进入这个区域时发出的。body 可以是一个 PhysicsBody2D 或一个 TileMap。如果 TileMap 的 TileSet 配置了碰撞形状，就会被检测到。需要将 monitoring 设置为 true 。

● body_exited(body: Node2D)当接收到的 body 离开这个区域时发出的。body 可以是一个 PhysicsBody2D 或一个 TileMap。如果 TileMap 的 TileSet 配置了碰撞形状，就会被检测到。要求 monitoring 被设置为 true 。

● body_shape_entered(body_rid: RID, body: Node2D, body_shape_index: int, local_shape_index: int)当收到的 body 的 Shape2D 进入这个区域的形状时发出。body 可以是一个 PhysicsBody2D 或一个 TileMap。如果 TileMap 的 TileSet 配置了碰撞形状，则会被检测到。要求 monitoring 被设置为 true 。

local_shape_index 和 body_shape_index 分别包含来自这个区域和交互体的交互形状的指数。body_rid 包含体的 RID。这些值可以与 PhysicsServer2D 一起使用。

从形状索引中获取 CollisionShape2D 节点的例子。

var body_shape_owner = body.shape_find_owner(body_shape_index)
var body_shape_node = body.shape_owner_get_owner(body_shape_owner)

var local_shape_owner = shape_find_owner(local_shape_index)
var local_shape_node = shape_owner_get_owner(local_shape_owner)

● body_shape_exited(body_rid: RID, body: Node2D, body_shape_index: int, local_shape_index: int)当收到的 body 的 Shape2D 退出这个区域的形状时发出。body 可以是一个 PhysicsBody2D 或一个 TileMap。如果 TileMap 的 TileSet 配置了碰撞形状，则会被检测到。要求 monitoring 被设置为 true 。

另请参阅 body_shape_entered。


枚举
enum  SpaceOverride:

● SPACE_OVERRIDE_DISABLED = 0
这个区域不影响重力/阻尼。
● SPACE_OVERRIDE_COMBINE = 1
该区域将其重力/阻尼值加到迄今为止计算出的任何值上（按 priority 排序）。
● SPACE_OVERRIDE_COMBINE_REPLACE = 2
该区域将其重力/阻尼值添加到到目前为止已计算的任何内容（按 priority 顺序），而忽略任何较低优先级的区域。
● SPACE_OVERRIDE_REPLACE = 3
该区域将替换所有重力/阻尼，甚至是默认值，而忽略任何较低优先级的区域。
● SPACE_OVERRIDE_REPLACE_COMBINE = 4
这个区域取代了到目前为止计算出的任何重力/阻尼（按 priority 顺序），但继续计算其余的区域。


属性说明

● float angular_damp [默认： 1.0]set_angular_damp(值) setterget_angular_damp() getter

物体在此区域停止旋转的速度。代表每秒损失的角速度。

关于阻尼的更多细节见 ProjectSettings.physics/2d/default_angular_damp。


● SpaceOverride angular_damp_space_override [默认： 0]set_angular_damp_space_override_mode(值) setterget_angular_damp_space_override_mode() getter

此区域内的角阻尼计算的覆盖模式。有关可能的值，请参阅 SpaceOverride。


● StringName audio_bus_name [默认： &"Master"]set_audio_bus_name(值) setterget_audio_bus_name() getter

该区域音频总线的名称。


● bool audio_bus_override [默认： false]set_audio_bus_override(值) setteris_overriding_audio_bus() getter

如果为 true，该区域的音频总线将覆盖默认的音频总线。


● float gravity [默认： 980.0]set_gravity(值) setterget_gravity() getter

该区域的重力强度（以像素每平方秒为单位）。这个值是重力向量的倍数。这对于改变重力大小而不改变其方向很有用。


● Vector2 gravity_direction [默认： Vector2(0, 1)]set_gravity_direction(值) setterget_gravity_direction() getter

该区域的重力向量（未归一化）。


● bool gravity_point [默认： false]set_gravity_is_point(值) setteris_gravity_a_point() getter

如果为 true，则从一个点（通过 gravity_point_center 设置）计算重力。参阅 gravity_space_override。


● Vector2 gravity_point_center [默认： Vector2(0, 1)]set_gravity_point_center(值) setterget_gravity_point_center() getter

如果重力是一个点（参见 gravity_point），这将是吸引力点。


● float gravity_point_unit_distance [默认： 0.0]set_gravity_point_unit_distance(值) setterget_gravity_point_unit_distance() getter

重力强度等于 gravity 的距离。例如，在一个半径为 100 像素、表面重力为 4.0 px/s² 的行星上，将 gravity 设置为 4.0，将单位距离设置为 100.0。重力将根据平方反比定律衰减，因此在该示例中，距离中心 200 像素处的重力将为 1.0 px/s²（距离的两倍，重力的 1/4），距离 50 像素处为 16.0 px/s²（距离的一半，重力的 4 倍），依此类推。

仅当单位距离为正数时，上述情况才成立。当该属性被设置为 0.0 时，无论距离如何，重力都将保持不变。


● SpaceOverride gravity_space_override [默认： 0]set_gravity_space_override_mode(值) setterget_gravity_space_override_mode() getter

该区域内重力计算的覆盖模式。有关可能的值，请参阅 SpaceOverride。


● float linear_damp [默认： 0.1]set_linear_damp(值) setterget_linear_damp() getter

物体在此区域停止运动的速度。代表每秒损失的线速度。

关于阻尼的更多细节见 ProjectSettings.physics/2d/default_linear_damp。


● SpaceOverride linear_damp_space_override [默认： 0]set_linear_damp_space_override_mode(值) setterget_linear_damp_space_override_mode() getter

该区域内线性阻尼计算的覆盖模式。可取的值见 SpaceOverride。


● bool monitorable [默认： true]set_monitorable(值) setteris_monitorable() getter

如果为 true，其他监测区域可以检测到这个区域。


● bool monitoring [默认： true]set_monitoring(值) setteris_monitoring() getter

为 true 时，该区域能够检测到进入和退出该区域的实体或区域。


● int priority [默认： 0]set_priority(值) setterget_priority() getter

该区域的优先级。将优先处理优先级较高的区域。World2D 的物理始终在所有区域之后处理。


方法说明

● Array[Area2D] get_overlapping_areas() const

返回相交的 Area2D 的列表。重叠区域的 CollisionObject2D.collision_layer 必须是这个区域 CollisionObject2D.collision_mask 的一部分，这样才能被检测到。

出于性能的考虑（所有碰撞都是一起处理的），这个列表会在物理步骤中进行一次修改，而不是在物体被移动后立即修改。可考虑改用信号。


● Array[Node2D] get_overlapping_bodies() const

返回相交的 PhysicsBody2D 和 TileMap。重叠物体的 CollisionObject2D.collision_layer 必须是该区域 CollisionObject2D.collision_mask 的一部分，才能被检测到。

出于性能原因（所有碰撞都是一起处理的），这个列表只会在每次物理迭代时发生一次更改，不会在对象移动后立即更改。请考虑使用信号。


● bool has_overlapping_areas() const

如果与其他 Area2D 相交，则返回 true，否则返回 false。重叠区域的 CollisionObject2D.collision_layer 必须是该区域 CollisionObject2D.collision_mask 的一部分，才能被检测到。

出于性能原因（所有碰撞都是一起处理的），重叠区域的列表只会在每次物理迭代时发生一次更改，不会在对象移动后立即更改。请考虑使用信号。


● bool has_overlapping_bodies() const

如果与其他 PhysicsBody2D 或 TileMap 相交，则返回 true，否则返回 false。重叠物体的 CollisionObject2D.collision_layer 必须是该区域 CollisionObject2D.collision_mask 的一部分，才能被检测到。

出于性能原因（所有碰撞都是一起处理的），重叠物体的列表只会在每次物理迭代时发生一次更改，不会在对象移动后立即更改。请考虑使用信号。


● bool overlaps_area(area: Node) const

如果给定的 Area2D 与此 Area2D 相交或重叠，则返回 true，否则返回 false。

注意：测试结果不反映对象移动后的即时状态。出于性能原因，重叠列表每帧只会在物理迭代前更新一次。请考虑使用信号。


● bool overlaps_body(body: Node) const

如果给定的物理物体与此 Area2D 相交或重叠，则返回 true，否则返回 false。

注意：测试结果不反映对象移动后的即时状态。出于性能原因，重叠列表每帧只会在物理迭代前更新一次。请考虑使用信号。

参数 body 可以是 PhysicsBody2D 实例，也可以是 TileMap 实例。TileMap 虽然不是物理物体，但会把图块的碰撞形状注册为虚拟物理物体。

---

---

## 类：   ParallaxBackground

继承：   CanvasLayer <   Node <   Object


用于创建视差滚动背景的节点。


描述

ParallaxBackground 使用一个或多个 ParallaxLayer 子节点来创建视差效果。每个 ParallaxLayer 可以使用 ParallaxLayer.motion_offset 以不同的速度移动。这在 2D 游戏中可以创造一种深度错觉。如果没有与 Camera2D 一起使用，你必须手动计算 scroll_offset。

注意：每个 ParallaxBackground 都是在各自的 Viewport 中绘制的，无法在不同 Viewport 之间共享，见 CanvasLayer.custom_viewport。在分屏游戏等使用多个 Viewport 的场景下，你需要每个需要绘制的 Viewport 创建单独的 ParallaxBackground。


属性
intlayer [覆盖 CanvasLayer： -100]Vector2scroll_base_offset [默认： Vector2(0, 0)]Vector2scroll_base_scale [默认： Vector2(1, 1)]boolscroll_ignore_camera_zoom [默认： false]Vector2scroll_limit_begin [默认： Vector2(0, 0)]Vector2scroll_limit_end [默认： Vector2(0, 0)]Vector2scroll_offset [默认： Vector2(0, 0)]

属性说明

● Vector2 scroll_base_offset [默认： Vector2(0, 0)]set_scroll_base_offset(值) setterget_scroll_base_offset() getter

所有 ParallaxLayer 子元素的基本位置偏移。


● Vector2 scroll_base_scale [默认： Vector2(1, 1)]set_scroll_base_scale(值) setterget_scroll_base_scale() getter

所有 ParallaxLayer 子元素的基本移动比例。


● bool scroll_ignore_camera_zoom [默认： false]set_ignore_camera_zoom(值) setteris_ignore_camera_zoom() getter

为 true 时，ParallaxLayer 子元素将不受相机缩放级别的影响。


● Vector2 scroll_limit_begin [默认： Vector2(0, 0)]set_limit_begin(值) setterget_limit_begin() getter

开始滚动的左上角限制。如果相机超出这个限制，背景将停止滚动。必须低于 scroll_limit_end 才能工作。


● Vector2 scroll_limit_end [默认： Vector2(0, 0)]set_limit_end(值) setterget_limit_end() getter

右下角限制滚动结束。如果相机超出这个限制，背景将停止滚动。必须高于 scroll_limit_begin 才能工作。


● Vector2 scroll_offset [默认： Vector2(0, 0)]set_scroll_offset(值) setterget_scroll_offset() getter

视差背景的滚动值。使用 Camera2D 时会自动计算，但也可用于手动管理无相机时的滚动。

---

---

## 类：   AnimatedSprite2D

继承：   Node2D <   CanvasItem <   Node <   Object


包含多个纹理作为动画播放帧的 Sprite 节点。


描述

AnimatedSprite2D 与 Sprite2D 节点类似，但是包含多张纹理，可用作动画帧。动画使用 SpriteFrames 资源创建，可以导入图像文件（或包含此类文件的文件夹）为该精灵提供动画帧。可以在编辑器的“动画帧”底部面板中配置 SpriteFrames 资源。


在线教程
2D 精灵动画
2D Dodge The Creeps 演示


属性
StringNameanimation [默认： &"default"]Stringautoplay [默认： ""]boolcentered [默认： true]boolflip_h [默认： false]boolflip_v [默认： false]intframe [默认： 0]floatframe_progress [默认： 0.0]Vector2offset [默认： Vector2(0, 0)]floatspeed_scale [默认： 1.0]SpriteFramessprite_frames

方法
floatget_playing_speed() constboolis_playing() constvoidpause()voidplay(name: StringName = &"", custom_speed: float = 1.0, from_end: bool = false)voidplay_backwards(name: StringName = &"")voidset_frame_and_progress(frame: int, progress: float)voidstop()

信号

● animation_changed()当 animation 更改时发出。

● animation_finished()当动画到达结尾时，或者如果反向播放则到达起点时发出。当动画结束时，它会暂停播放。

● animation_looped()当动画循环播放时发出。

● frame_changed()frame 更改时发出。

● sprite_frames_changed()当 sprite_frames 更改时发出。


属性说明

● StringName animation [默认： &"default"]set_animation(值) setterget_animation() getter

当前动画，来自 sprite_frames。如果更改了这个值，会重置 frame 计数和 frame_progress。


● String autoplay [默认： ""]set_autoplay(值) setterget_autoplay() getter

场景加载时要播放的动画名称。


● bool centered [默认： true]set_centered(值) setteris_centered() getter

如果为 true，纹理将被居中。


● bool flip_h [默认： false]set_flip_h(值) setteris_flipped_h() getter

如果为 true，纹理将被水平翻转。


● bool flip_v [默认： false]set_flip_v(值) setteris_flipped_v() getter

如果为 true，纹理将被垂直翻转。


● int frame [默认： 0]set_frame(值) setterget_frame() getter

所显示动画帧的索引。设置这个属性会重置 frame_progress。如果不希望这样，请使用 set_frame_and_progress()。


● float frame_progress [默认： 0.0]set_frame_progress(值) setterget_frame_progress() getter

当前帧过渡到下一帧的进度值，在 0.0 和 1.0 之间。如果动画是倒放的，则该值是从 1.0 到 0.0。


● Vector2 offset [默认： Vector2(0, 0)]set_offset(值) setterget_offset() getter

纹理的绘图偏移量。


● float speed_scale [默认： 1.0]set_speed_scale(值) setterget_speed_scale() getter

速度缩放比。例如，如果该值为 1，则动画以正常速度播放。如果它是 0.5，那么它会半速播放。如果是 2，则会以双倍速度播放。

如果设置为负值，则动画反向播放。如果设置为0，则动画不会前进。


● SpriteFrames sprite_framesset_sprite_frames(值) setterget_sprite_frames() getter

包含动画的 SpriteFrames 资源。可以对 SpriteFrames 资源进行加载、编辑、清空、唯一化、保存状态等操作。


方法说明

● float get_playing_speed() const

返回当前动画的实际播放速度，未播放时则为 0。这个速度是 speed_scale 属性乘以调用 play() 时指定的 custom_speed 参数。

如果当前动画是倒放的，则返回负值。


● bool is_playing() const

如果动画目前正在播放，则返回 true（即便 speed_scale 和/或 custom_speed 为 0）。


● void pause()

暂停当前正在播放的动画。会保留 frame 和 frame_progress，不带参数调用 play() 或 play_backwards() 会从当前播放位置恢复播放该动画。

另见 stop()。


● void play(name: StringName = &"", custom_speed: float = 1.0, from_end: bool = false)

播放名称键为 name 的动画。如果 custom_speed 为负且 from_end 为 true，则该动画会倒放（等价于 play_backwards()）。

如果调用这个方法时使用了相同的动画名称 name 或者没有使用 name 参数，则会继续播放已暂停的分配动画。


● void play_backwards(name: StringName = &"")

倒放名称键为 name 的动画。

这个方法是简写，等价于调用 play() 时使用 custom_speed = -1.0 和 from_end = true，所以更多信息请参阅其描述。


● void set_frame_and_progress(frame: int, progress: float)

设置 frame 时会隐式将 frame_progress 重置为 0.0，但这个方法可以避免。

如果你想要把当前的 frame_progress 带到其他 frame 中，就会非常有用。

示例：

==更改动画的同时保留帧索引和进度。==

var current_frame = animated_sprite.get_frame()
var current_progress = animated_sprite.get_frame_progress()
animated_sprite.play("walk_another_skin")
animated_sprite.set_frame_and_progress(current_frame, current_progress)


● void stop()

停止当前正在播放的动画。会将动画的位置重置为 0，并将 custom_speed 重置为 1.0。另见 pause()。

---

---

## 类：   CollisionShape2D

继承：   Node2D <   CanvasItem <   Node <   Object


向 CollisionObject2D 父级提供 Shape2D 的节点。


描述

向 CollisionObject2D 父级提供 Shape2D 并允许对其进行编辑的节点。这可以为 Area2D 提供检测形状或将 PhysicsBody2D 转变为实体对象。


在线教程
物理介绍
2D Dodge The Creeps 演示
2D Pong 演示
2D 运动学角色演示


属性
Colordebug_color [默认： Color(0, 0, 0, 1)]booldisabled [默认： false]boolone_way_collision [默认： false]floatone_way_collision_margin [默认： 1.0]Shape2Dshape

属性说明

● Color debug_color [默认： Color(0, 0, 0, 1)]set_debug_color(值) setterget_debug_color() getter

碰撞形状的调试颜色。

注意：默认值为 ProjectSettings.debug/shapes/collision/shape_color。这里记录的 Color(0, 0, 0, 1) 值是占位符，不是实际的默认调试颜色。


● bool disabled [默认： false]set_disabled(值) setteris_disabled() getter

禁用的碰撞形状在世界中没有影响。这个属性应该用 Object.set_deferred() 改变。


● bool one_way_collision [默认： false]set_one_way_collision(值) setteris_one_way_collision_enabled() getter

设置此碰撞形状是否仅应检测到一侧（顶部或底部）的碰撞。

注意：如果这个 CollisionShape2D 是 Area2D 节点的子节点，则这个属性无效。


● float one_way_collision_margin [默认： 1.0]set_one_way_collision_margin(值) setterget_one_way_collision_margin() getter

用于单向碰撞的边距（以像素为单位）。较高的值将使形状更厚，并且对于高速进入形状的对撞机来说效果更好。


● Shape2D shapeset_shape(值) setterget_shape() getter

该碰撞形状拥有的实际形状。