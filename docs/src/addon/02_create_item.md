## 注册物品

### 打开 `init.ModItems.java` ，你将看到如下语句：

* ```java
  public static final ItemEntry<Item> EXAMPLE_ITEM = REGISTRATE
    .item("example_item", Item::new)
    .register();
  ```
  该语句即为注册物品的示例，其中 `example_item` 为你即将注册的物品的ID，`Item::new` 为你物品类构造方法的引用。
* ```java
  static {
      REGISTRATE.creativeModeTab(ModCreativeModeTab.EXAMPLE_TAB);
  }
  ```
  该代码块代表在此之后注册的物品将添加至指定的创造模式物品栏
* ```java
  public static void register() {}
  ```
  该空方法用以加载此类

### 本章节内容将详细介绍 `REGISTRATE.item()` 的使用方法

* 使用 `REGISTRATE.item()` 方法后，你将拿到一个 `ItemBuilder` ，该对象拥有一个 `.register()` 方法，调用后返回一个 `ItemEntry` ，其对应的物品将在合适的时机自动注册，后文将着重介绍 `ItemBuilder` 与其所具备的方法。
* `ItemBuilder.properties()`
  * 该方法用于修改物品的默认属性，可以接受一个无返回值的 lambda 表达式，该表达式将当前物品属性作为参数传入
  * 示例用法：
    ```java
    public static final ItemEntry<Item> EXAMPLE_ITEM = REGISTRATE
        .item("example_item", Item::new)
        .properties(prop->prop.durability(15))
        .register();
    ```
    该示例展示了如何为注册的物品设置最大耐久值
* `ItemBuilder.initialProperties()`
  * 该方法用于设置物品的默认属性，接受一个返回 `Item.Properties` 的无参 lambda 表达式，返回值将作为该物品的默认物品属性
  * 示例用法：
    ```java
    public static final ItemEntry<Item> EXAMPLE_ITEM = REGISTRATE
        .item("example_item", Item::new)
        .initialProperties(Item.Properties::new)
        .register();
    ```
    该示例展示了如何为注册的物品设置物品默认属性，注意，你通常不需要这么做，该行为已在 `REGISTRATE` 中定义，该实例仅作为教学示范
* `ItemBuilder.tab()` 、 `ItemBuilder.removeTab()`
  * 我们不建议使用该方法设置创造模式物品栏，你应该使用文章开头的方法
* `ItemBuilder.color()`
  * 为该物品注册颜色处理器，对于大多数物品，你不需要使用该方法，特别的，如果你想制作类似药水的物品，该方法则很有帮助
  * 示例用法：
    ```java
    public static final ItemEntry<Item> EXAMPLE_ITEM = REGISTRATE
        .item("example_item", Item::new)
        .color(() -> () -> (itemStack, i) -> 0xFFFFFFFF)
        .register();
    ```
    该用法仅供学习 ~~（过于抽象，不好评价）~~ ，返回值为颜色的 ARGB 值 ~~（为什么alpha在最前）~~
* `ItemBuilder.model()`
  * 该方法用于设定物品的模型生成器
  * ```java
    public static final ItemEntry<Item> EXAMPLE_ITEM = REGISTRATE
        .item("example_item", Item::new)
        .model((ctx, provider) -> provider.handheld(ctx))
        .register();
    ```
    该示例展示了如何为物品设置手持物品父模型（例如各种工具）的模型生成器
* `ItemBuilder.lang()`
  * 该方法用于设定物品的默认显示名称，未指定时，将自动使用注册ID转大驼峰加空格作为显示名称，该方法会生成 `en_us` 与 `en_ud（倒置英语）` 语言文件
  * ```java
    public static final ItemEntry<Item> EXAMPLE_ITEM = REGISTRATE
        .item("example_item", Item::new)
        .lang("Item Example")
        .register();
    ```
    该示例展示了如何将注册物品的默认显示名称修改为 `Item Example`
* `ItemBuilder.recipe()`
  * 该方法用于设置物品的配方生成器
  * ```java
    public static final ItemEntry<Item> EXAMPLE_ITEM = REGISTRATE
        .item("example_item", Item::new)
        .recipe((ctx, provider) -> ShapedRecipeBuilder.shaped(RecipeCategory.MISC, ctx.get())
            .pattern("AB")
            .pattern("BA")
            .define('A', Items.IRON_INGOT)
            .define('B', Items.COPPER_INGOT)
            .unlockedBy(AnvilCraftDatagen.hasItem(Items.IRON_INGOT), RegistrateRecipeProvider.has(Items.IRON_INGOT))
            .unlockedBy(AnvilCraftDatagen.hasItem(Items.COPPER_INGOT), RegistrateRecipeProvider.has(Items.COPPER_INGOT))
            .save(provider)
        )
        .register();
    ```
    该示例展示了如何为物品生成一个有序合成的配方，同时还将生成对应的解锁进度
* `ItemBuilder.tag()`
  * 该方法用于设置物品的标签生成器
  * ```java
    public static final ItemEntry<Item> EXAMPLE_ITEM = REGISTRATE
        .item("example_item", Item::new)
        .tag(ItemTags.AXES)
        .register();
    ```
    该示例展示如何将物品加入至原版斧子标签内
