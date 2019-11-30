> **目录结构** 

```
|－ ecs
|     |－ ecs.js 全局变量
|     |－ Component.ecs.js 组件基类
|     |－ ComponentManager.ecs.js 组件管理器
|     |－ Entity.ecs.js 实体基类
|     |－ EntityManager.ecs.js 实体管理器
|     |－ System.ecs.js 系统基类
|     |－ SystemManager.ecs.js 系统管理器
|     |－ Input.ecs.js 输入组件
|     |－ Mediator.ecs.js 系统消息中介
|     |－ worker.ecs.js web worker
|     |－ Pool.ecs.js 对象池
```

&nbsp;

##  全局变量 - ECS

| File | Path | Description | Versions |
| :--------: | :--------: | -------- | :--------: |
| ecs.js | src/ecs/ | 全局变量 | 1.0.0 |

| Properties | Type | Default | Description |
| :--------: | :--------: | :--------: | :--------: |
| worker | object | - | ecs全局变量 |
| mediator | object | - | 系统消息中介 |
| input | object | - | 输入组件 |
| componentManager | object | - | 组件管理器 |
| Component | object | - | 组件基类 |
| entityManager | object | - | 实体管理器 |
| Entity | object | - | 实体基类 |
| componentManager | object | - | 组件管理器 |
| Component | object | - | 组件基类 |

| Methods | Parameter | Type | Return | Type | Description |
| :--------: | :--------: | :--------: | :--------: |:--------: | :--------: |
| update(dt) | dt 帧间隔时间 | number | - | - | 更新系统 |
| send(systemClass, data) | systemClass 系统类<br />systemClass 系统名<br />data 数据 | function<br />string<br />object | - | - | 发送消息 |
| getClassName(ecsClass) | ecsClass 类<br />ecsClass 类名 | function<br />string | className 类名 | string | 获取类名 |
| getUUID() | - | - | uuid 唯一标识 | string | 获取uuid |

| Example |
| :-------- |
```javaScript
// 更新系统
export default class Game extends Scene {
    constructor() {
        super();
    }

    update(dt) {
        ecs.update(dt);
    }
}

// 获取类名
const className = ecs.getClassName(Game);

// 获取uuid
const uniqueId = ecs.getUUID();
``` 

&nbsp;

##  输入组件 - Input

| File | Path | Description | Versions |
| :--------: | :--------: | -------- | :--------: |
| input.ecs.js | src/ecs/ | 全局变量 | 1.0.0 |

| Properties | Type | Default | Description |
| :--------: | :--------: | :--------: | :--------: |
| _ecs | object | - | ecs全局变量 |
| _codeMap | object | - | code集合 |
| _keyCode | object | - | keyCode |
| _inputMap | object | - | 输入集合 |

| Methods | Parameter | Type | Return | Type | Description |
| :--------: | :--------: | :--------: | :--------: |:--------: | :--------: |
| keyCode | obj keycode | object | - | - | 获取/设置keycode |
| get(code) | code keycode | string | data 输入数据 | object | 获取输入数据 |
| getAll() | - | - | inputMap 输入集合 | object | 获取所有输入数据 |
| set(code, data) | code keycode<br />data 输入数据 | string<br />object | - | - | 设置输入数据 |
| has(code) | code keycode | string | isExist keycode是否存在 | boolean | 检测keycode是否存在 |
| remove(code) | code keycode | string | - | - | 移除keycode和输入数据 |
| clear() | - | - | - | - | 清空keycode和输入数据 |

| Example |
| :-------- |
```javaScript
// 设置keycode
ecs.input.keyCode = keyCode;

// 设置输入数据
const move = (keyCode) => {
    ecs.input.set(keyCode, { id: 0 });
}

const btnRight = this._addBtn('controller-right.png', {
    right: 16,
    centerY: 0
}, () => {
    move(ecs.input.keyCode.RIGHT);
});

// 检测keycode是否存在ke和获取输入数据
onUpdate(dt) {
    let data = null;
    let x = 0;
    let y = 0;
    const playerMap = this._ecs.entityManager.getMap('Player');

    if (this._ecs.input.has(this._ecs.input.keyCode.TOP)) {
        data = this._ecs.input.get(this._ecs.input.keyCode.TOP);

        y -= 4;
    }

    if (this._ecs.input.has(this._ecs.input.keyCode.BOTTOM)) {
        data = this._ecs.input.get(this._ecs.input.keyCode.BOTTOM);

        y += 4;
    }

    if (this._ecs.input.has(this._ecs.input.keyCode.LEFT)) {
        data = this._ecs.input.get(this._ecs.input.keyCode.LEFT);

        x -= 4;
    }

    if (this._ecs.input.has(this._ecs.input.keyCode.RIGHT)) {
        data = this._ecs.input.get(this._ecs.input.keyCode.RIGHT);

        x += 4;
    }
}
``` 

&nbsp;

##  组件基类 - Component

| File | Path | Description | Versions |
| :--------: | :--------: | -------- | :--------: |
| Component.ecs.js | src/ecs/ | 组件是一堆数据的集合，它没有方法，即不存在任何的行为，只用来存储状态。一个经典的实现是：每一个组件都继承（或实现）同一个基类（或接口），通过这样的方法，我们能够非常方便地在运行时动态添加、识别、移除组件。每一个组件的意义在于描述实体的某一个特性。 | 1.0.0 |

| Properties | Type | Default | Description |
| :--------: | :--------: | :--------: | :--------: |
| _ecs | object | - | ecs全局变量 |

| Methods | Parameter | Type | Description |
| :--------: | :--------: | :--------: | :--------: |
| - | - | - | - |

| Example |
| :-------- |
```javaScript
class Position extends ecs.Component {
	static get name() {
        return 'Position';
    }
    
    constructor(x, y) {
        super();

        this.x = x;
    	this.y = y;
    }
}
``` 

&nbsp;

##  组件管理器 - Component Manager

| File | Path | Description | Versions |
| :--------: | :--------: | -------- | :--------: |
| ComponentManager.ecs.js | src/ecs/ | 提供方法操作组件集合 | 1.0.0 |

| Properties | Type | Default | Description |
| :--------: | :--------: | :--------: | :--------: |
| _ecs | object | - | ecs全局变量 |
| _compMaps | object | - | 组件集合 |

| Methods | Parameter | Type | Return | Type | Description |
| :--------: | :--------: | :--------: | :--------: |:--------: | :--------: |
| _preRegister(compClass) | compClass 组件类 | function | isRegister 是否已注册 | boolean | 检测组件是否已注册 |
| registery(compClass) | compClass 组件类 | function | - | - | 注册组件 |
| getMap(compClass) |compClass 组件类<br />compClass 组件名 | function<br />string | compMap 当前组件集合 | object | 获取组件集合 |
| get(compClass, entityId) | compClass 组件类<br />compClass 组件名<br />entityId 实体ID | function<br />string<br />string | comp 当前组件 | object | 获取组件 |
| getAll(entityId) | entityId 实体ID | string | comps 所有组件 | array | 获取所有组件 |
| set(comp, entityId) | comp 组件实例<br />entityId 实体ID | object<br />string | - | - | 添加组件，自动注册 |
| has(compClass, entityId) | compClass 组件类<br />compClass 组件名<br />entityId 实体ID | function<br />string<br />string | isExist 组件是否存在 | boolean | 检测组件是否存在 |
| remove(compClass, entityId) | compClass 组件类<br />compClass 组件名<br />entityId 实体ID | function<br />string<br />string | - | - | 移除组件 |
| clear(entityId) | entityId 实体ID | string | - | - | 清空组件 |
| clearAll() | - | - | - | - | 清空所有组件 |
| _destroy(compMap, entityId) | compMap 组件集合<br />entityId 实体ID | object<br />string | - | - | 销毁组件 |

| Example |
| :-------- |
```javaScript
// 注册组件
ecs.componentManager.registery(Position);

// 获取组件类集合
ecs.componentManager.getMap(Position);

// 获取组件
ecs.componentManager.get(Position, entityId);

// 获取所有组件
ecs.componentManager.getAll(entityId);

// 添加组件
const position = new Position(x, y);
ecs.componentManager.set(position, entityId);

// 检测组件是否存在
ecs.componentManager.has(Position, entityId);

// 移除组件
ecs.componentManager.remove(Position, entityId);

// 清空组件
ecs.componentManager.clear(entityId);

// 清空所有组件
ecs.componentManager.clearAll();
``` 

&nbsp;

##  实体基类 - Entity

| File | Path | Description | Versions |
| :--------: | :--------: | -------- | :--------: |
| Entity.ecs.js | src/ecs/ | 实体只是一个概念上的定义，指的是存在你游戏世界中的一个独特物体，一个基本单元，是一系列组件的集合。为了方便区分不同的实体，在代码层面上一般用一个ID来进行表示。所有组成这个实体的组件将会被这个ID标记，从而明确哪些组件属于该实体。由于其是一系列组件的集合，因此完全可以在运行时动态地为实体增加一个新的组件或是将组件从实体中移除。 | 1.0.0 |

| Properties | Type | Default | Description |
| :--------: | :--------: | :--------: | :--------: |
| _ecs | object | - | ecs全局变量 |
| _name | string | 'default' | 名称 |
| _uniqueId | string | - | 唯一标识 |

| Methods | Parameter | Type | Return | Type | Description |
| :--------: | :--------: | :--------: | :--------: |:--------: | :--------: |
| name | - | - | name 名称 | string | 获取名称 |
| id | - | - | id ID | string | 获取ID |
| destroy() | - | - | - | - | 销毁 |
| getComp(compClass) | compClass 组件类<br />compClass 组件名 | function<br />string | comp 组件 | object | 获取组件 |
| getComps() | - | - | comps 所有组件 | array | 获取所有组件 |
| setCompsState(compClass, state, hasDiff = true) | compClass 组件类<br />compClass 组件名<br />state 状态<br />hasDiff 是否开启比对 | function<br />string<br />object<br />boolean | - | - | 设置组件状态 |
| hasComp(compClass) | compClass 组件类<br />compClass 组件名 | function<br />string | isExist 是否存在 | boolean | 检测组件是否存在 |
| addComp(comp) | comp 组件实例 | object | this chain | object | 添加组件 |
| removeComp(compClass) | compClass 组件类<br />compClass 组件名 | function<br />string | this chain | object | 移除组件 |
| clearComps() | - | - | this chain | object | 清空组件 |

| Example |
| :-------- |
```javaScript
const position = new Components.Position(x, y);

// 创建实体，添加组件
const entity = new ecs.Entity('Player').addComp(Position);

// 获取名称
entity.name

// 获取ID
entity.id

// 销毁实体
entity.destroy();

// 获取组件
entity.getComp(Position);

// 获取所有组件
entity.getComps();

// 设置组件状态
entity.setCompsState(Tween, {
    x: x,
    y: y
});

// 检测组件是否存在
entity.hasComp(Position);

// 移除组件
entity.removeComp(Position);

// 清空组件
entity.clearComps(Position);
``` 

&nbsp;

##  实体管理器 - Entity Manager

| File | Path | Description | Versions |
| :--------: | :--------: | -------- | :--------: |
| entityManager.ecs.js | src/ecs/ | 提供方法操作实体集合 | 1.0.0 |

| Properties | Type | Default | Description |
| :--------: | :--------: | :--------: | :--------: |
| _ecs | object | - | ecs全局变量 |
| _entityMaps | object | - | 实体集合 |

| Methods | Parameter | Type | Return | Type | Description |
| :--------: | :--------: | :--------: | :--------: |:--------: | :--------: |
| _preRegister(entityName) | entityName 实体名 | string | isRegister 是否已注册 | boolean | 检测实体是否已注册 |
| registery(entityName) | entityName 实体名 | string | this chain | object | 注册实体 |
| registery(entityNames) | entityNames 实体名 | array | this chain | object | 批量注册实体 |
| getMap(entityName) |entityName 实体名 | string | entityMap 当前实体集合 | object | 获取实体集合 |
| get(entityName, entityId) | entityName 实体名<br />entityId 实体ID | string<br />string | entitys 实体 | array | 根据实体ID，获取实体 |
| filter(entityName, compClasses) | entityName 实体名<br />compClasses 组件类 | string<br />array | entity 当前实体 | array | 根据组件类，获取实体 |
| first(entityName) | entityName 实体名 | string | entity 当前实体 | array | 获取第一个实体 |
| find(entityName, compClass, data) | entityName 实体名<br />compClasses 组件类<br />data 数据 | string<br />function<br />object | entity 当前实体 | object | 根据条件查找实体 |
| set(entity) | entity 组件实例 | object | entity 实体 | object | 添加实体，自动注册 |
| has(entityName, entityId) | entityName 实体名<br />entityId 实体ID | string<br />string | isExist 实体是否存在 | boolean | 检测实体是否存在 |
| hasEntity(entity | entity 组件实例 | object | isExist 实体是否存在 | boolean | 检测实体是否存在 |
| destroy(entityName, entityId) | entityName 实体名<br />entityId 实体ID | string<br />string | this chain | object | 销毁实体 |
| destroyEntity(entity) | entity 组件实例 | object | this chain | object | 销毁实体 |
| clear() | - | - | this chain | object | 清空实体 |
| getDirty() | - | - | dirty 脏标记数据 | object | 获取脏标记 |
| setDirty(entityTag, compName, attrs = 'all') | entityTag 实体标识（entityName@entityId）<br />compName 组件名称<br />attrs 属性 | string<br />string<br/ >object | - | - | 设置脏标记 |
| clearDirty() | - | - | - | - | 清空脏标记 |

| Example |
| :-------- |
```javaScript
// 注册实体
const player = new ecs.Entity('Player')

ecs.entityManager.registery('Player');

// 批量注册实体
ecs.entityManager.registeryMany(['Player', 'Tile']);

// 获取实体类集合
ecs.entityManager.getMap('Player');

// 根据实体ID，获取实体
ecs.entityManager.get('Player', entityId);

// 根据组件名，获取实体
ecs.entityManager.filter('Player', [Player, Position]);

// 获取第一个实体
ecs.entityManager.first('Player');

// 根据条件查找实体
ecs.entityManager.find('Player', Position, { x: 0, y: 0 });

// 添加实体
ecs.entityManager.set(player);

// 检测实体是否存在
ecs.entityManager.has('Player', entityId);
ecs.entityManager.hasEntity(player);

// 销毁实体
ecs.entityManager.destroy(‘Player', entityId);
ecs.entityManager.destroyEntity(player);

// 清空实体
ecs.entityManager.clear(entityId);

// 设置脏标记
ecs.entityManager.setDirty(`${this._name}@${this._uniqueId}`, comp.constructor.name);

// 获取脏标记
ecs.entityManager.getDirty();

// 清空脏标记
ecs.entityManager.clearDirty();
``` 

&nbsp;

##  系统基类 - System

| File | Path | Description | Versions |
| :--------: | :--------: | -------- | :--------: |
| System.ecs.js | src/ecs/ | 系统便是ECS架构中用来处理游戏逻辑的部分。何为系统，一个系统就是对拥有一个或多个相同组件的实体集合进行操作的工具，它只有行为，没有状态，即不应该存放任何数据。 | 1.0.0 |

| Properties | Type | Default | Description |
| :--------: | :--------: | :--------: | :--------: |
| _ecs | object | - | ecs全局变量 |
| _world | object | - | 游戏世界 |
| _started | boolean | false | 运行状态 |
| _locked | boolean | false | 锁定状态 |
| _enabled | boolean | true | 激活状态 |

| Methods | Parameter | Type | Return | Type | Description |
| :--------: | :--------: | :--------: | :--------: |:--------: | :--------: |
| started | - | - | isStarted 运行状态 | boolean | 获取/设置运行状态 |
| enabled | - | - | isEnabled 激活状态 | boolean | 获取/设置激活状态 |
| initialize() | - | - | - | - | 系统内部初始化 |
| uninitialize() | - | - | - | - | 系统内部卸载 |
| update(dt) | dt 帧间隔时间 | number | - | - | 系统内部更新 |
| lateUpdate(dt) | dt 帧间隔时间 | number | - | - | 系统内部更新，在update之后执行 |
| receive(data) | data 接收数据 | object | - | - | 系统内部收到消息时调用 |
| onLoad() | - | - | - | - | 系统初始化时调用 |
| onStart() | - | - | - | - | 系统开始运行时调用 |
| onUpdate(dt) | dt 帧间隔时间 | number | - | - | 系统更新时调用 |
| onLateUpdate(dt) | dt 帧间隔时间 | number | - | - | 系统更新时调用 |
| onEnable() | - | - | - | - | 系统被激活时调用 |
| onDisable() | - | - | - | - | 系统被禁用时调用 |
| onDestroy() | - | - | - | - | 系统被注销时调用 |
| onReceive() | data 接收数据 | object | - | - | 系统收到消息时调用 |

| Example |
| :-------- |
```javaScript
import Components from '../../../game/components.game.js';

class PlayerSpawnSystem extends ecs.System {
	// 重置system.constructor.name, 避免代码压缩时，name发生变化
	static get name() {
        return 'PlayerSpawnSystem';
    }
    
    // 实例化时传入游戏世界
    constructor(world) {
        super();

        this._world = world;
    }

    onLoad() {

    }
	
	// 系统更新时调用
    onUpdate(dt) {

    }
	
	// 系统收到消息时调用
    onReceive({
    	id,
    	x,
    	y,
    	texture
    }) {
    	this.spawnPlayer(id, x, y, texture);
    }
	
	// 创建玩家实体，添加组件
    spawnPlayer(id, x, y, texture) {
    	const playerState = new Components.PlayerState(id);
    	const playerPosition = new Components.Position(x, y);
    	const playerSkin = new Components.Skin(texture);

    	const playerEntity = new ecs.Entity('Player')
    		.addComp(playerState)
    		.addComp(playerSkin)
    		.addComp(playerPosition);

    	this._world.addPlayer(playerEntity.id, x, y, texture);
    }
}

export default PlayerSpawnSystem;
``` 

&nbsp;

##  系统管理器 - System Manager

| File | Path | Description | Versions |
| :--------: | :--------: | -------- | :--------: |
| SystemManager.ecs.js | src/ecs/ | 提供方法操作系统集合 | 1.0.0 |

| Properties | Type | Default | Description |
| :--------: | :--------: | :--------: | :--------: |
| _ecs | object | - | ecs全局变量 |
| _systemMap | object | - | 系统集合 |
| _currentSystem | object | - | 当前运行系统 |

| Methods | Parameter | Type | Return | Type | Description |
| :--------: | :--------: | :--------: | :--------: |:--------: | :--------: |
| _preRegister(systemClass) | systemClass 系统类 | function | isRegister 是否已注册 | boolean | 检测系统是否已注册 |
| registery(system) | system 系统实例 | object | - | - | 注册系统 |
| registerBefore(system, beforeSystemClass) | system 系统实例<br />beforeSystemClass 系统类 | object<br />function | - | - | 注册系统，并添加到指定系统之前，如果没有指定，则添加到系统列表首部 |
| registerAfter(system, afterSystemClass) | system 系统实例<br />afterSystemClass 系统类 | object<br />function | - | - | 注册系统，并添加到指定系统之前，如果没有指定，则添加到系统列表尾部 |
| get(systemClass) | systemClass 系统类<br />systemClass 系统名 | function<br />string | system 当前系统 | object | 获取系统 |
| getAll() | - | - | systems 所有系统 | array | 获取所有系统 |
| has(systemClass) | systemClass 系统类<br />systemClass 系统名 | function<br />string | isExist 系统是否存在 | boolean | 检测系统是否存在 |
| enable(systemClass) | systemClass 系统类<br />systemClass 系统名 | function<br />string | - | - | 激活系统 |
| disable(systemClass) | systemClass 系统类<br />systemClass 系统名 | function<br />string | - | - | 禁用系统 |
| remove(systemClass) | systemClass 系统类<br />systemClass 系统名 | function<br />string | - | - | 移除系统 |
| clear() | - | - | - | - | 清空系统 |
| update(dt) | dt 帧间隔时间 | number | - | - | 更新系统 |

| Example |
| :-------- |
```javaScript
// 注册实体
const playerSpawnSystem = new PlayerSpawnSystem(this);
const playerMovementSystem = new PlayerMovementSystem(this);

ecs.systemManager.register(playerSpawnSystem);
ecs.systemManager.registerBefore(playerMovementSystem, playerSpawnSystem);
ecs.systemManager.registerAfter(playerMovementSystem, playerSpawnSystem);

// 获取系统
ecs.systemManager.get(PlayerMovementSystem);

// 获取所有系统
ecs.systemManager.getAll();

// 检测系统是否存在
ecs.systemManager.has(PlayerMovementSystem);

// 激活系统
ecs.systemManager.enable(PlayerMovementSystem);

// 禁用系统
ecs.systemManager.disable(PlayerMovementSystem);

// 移除系统
ecs.systemManager.clear(PlayerMovementSystem);

// 更新系统
ecs.systemManager.update(dt);
``` 

&nbsp;

##  对象池 - Pool

| File | Path | Description | Versions |
| :--------: | :--------: | -------- | :--------: |
| Pool.ecs.js | src/ecs/ | 对象池，用于对象的存储、重复使用。 | 1.0.0 |

| Properties | Type | Default | Description |
| :--------: | :--------: | :--------: | :--------: |
| _ecs | object | - | ecs全局变量 |
| _poolMap | object | - | 对象池集合 |
| _maxCount | number | 600 | 允许缓存的最大数量 |

| Methods | Parameter | Type | Return | Type | Description |
| :--------: | :--------: | :--------: | :--------: |:--------: | :--------: |
| resize | val 缓存对象数量 | number | - | - | 设置对象池容量 |
| get(sign) | sign 标识 | string | item 对象 | object | 获取对象池中的对象，如果没有，则返回null |
| getByClass(sign, cls) | sign 标识<br />cls 类 | string<br />function | item 对象 | object | 获取对象池中的对象，如果没有，则返回创建一个新的对象 |
| put(sign, item) | sign 标识<br />item 对象 | string<br />object | - | - | 回收对象到对象池 |
| size(sign) | sign 标识 | string | nums 可用对象数量 | number | 获取当前缓冲池的可用对象数量 |
| getPool(sign) | sign 标识 | string | pool 对象池 | array | 获取对象池，如果没有，则创建新对象池 |
| clearBySign(sign) | sign 标识 | string | - | - | 清除对象池的对象，并移除对象池 |
| clear() | - | - | - | - | 清楚所有对象池的对象，并移除所有对象池 |

| Example |
| :-------- |
```javaScript
// 设置对象池容量
ecs.pool.resize = 1000;

// 获取对象池中的对象，如果没有，则返回null
const player = ecs.pool.get('player');

// 获取对象池中的对象，如果没有，则返回创建一个新的对象
const player = ecs.pool.get('player', Player);

// 回收对象到对象池
ecs.pool.put('player', player);

// 获取对象池，如果没有，则创建新对象池
ecs.pool.getPool('player');

// 清除对象池的对象，并移除对象池
ecs.pool.clearBySign('player');

// 清楚所有对象池的对象，并移除所有对象池
ecs.pool.clear();
``` 
