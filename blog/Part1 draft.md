# My First Web Game with LoopBack4
## Part 1


### Instruction
In this series, I am going to create a API web game by using LoopBack4. This game is an online web Text-based Adventure Game.In this game, users can create their own characters, fight with monsters and find treasures. User can
control their character by calling specific APIs. Their actions may include attack enemy, cast spell, defeat enemy and get loot.

I don't have any background on web or game development. I graduated from college last year. The main purpose of this series is to show you how to learn LoopBack4 and how to use LoopBack4 to easily build your own API and web project.
I am sure most of you have better understanding than me on those fields. If I can do this, you can do better.

### Before we start
There are some prerequisite knowledge you may want to catch before we start.
* Basic concept of [Javascript](https://www.w3schools.com/js/) and [Node.js](https://www.w3schools.com/nodejs/nodejs_intro.asp)
* [Install LoopBack4](https://loopback.io/doc/en/lb4/Getting-started.html)
* I highly recommend you to check those two examples: [Todo tutorial](https://loopback.io/doc/en/lb4/todo-tutorial.html) and [TodoList tutorial](https://loopback.io/doc/en/lb4/todo-list-tutorial.html). This episode is base on thise examples. You don't have to understand how does that work. Just keep in mind what function we can achieve. We will dig deep into that later.

### In this episode
I will implement following features:
1. Create a character.
2. Create a weapon, armor, or skill.
3. Equip character.
4. Change weapon, armor and skill.
5. Update character information.
6. Levelup character.

There are four important components in a LB4 project: Model, Datasource, Repository, and Controller. Let's create them one by one.

First, let's create a LB4 project.
Run `lb4 app` in in a folder you want. This will create a new LB4 project.

Disable "Docker" when it ask you to "Select features to enable in the project"
```
wenbo:firstgameDemo wenbo$ lb4 app
? Project name: firstgame
? Project description: firstgameDemo
? Project root directory: firstgame
? Application class name: FirstgameApplication
? Select features to enable in the project Enable tslint, Enable prettier,
Enable mocha, Enable loopbackBuild, Enable vscode, Enable repositories, Ena
ble services
```

### Models
Then we need to create models. Model is like the class in Java or a table in relational database. It is a entity with one or more properties. A model may also has relationship with other models. For example, a `student` model could has properties like `studentID`, `name`, and `GPA`. It may also has one or more entity of `course` model and belong to a `school` model.

In this game, we want to let user create their own characters and equip their characters with weapons, armors, and skills. So models will looks like this.

![Models](https://github.com/gobackhuoxing/first-web-game-lb4/blob/master/picture/models.png)

Run `lb4 model` in your project folder.
```
? Model class name: character
? Please select the model base class Entity (A persisted model with an ID)
? Allow additional (free-form) properties? No
Let's add a property to Character
Enter an empty property name when done

? Enter the property name: id
? Property type: number
? Is id the ID property? Yes
? Is it required?: No
? Default value [leave blank for none]:

Let's add another property to Character
Enter an empty property name when done

? Enter the property name: name
? Property type: string
? Is it required?: Yes
? Default value [leave blank for none]:

Let's add another property to Character
Enter an empty property name when done

? Enter the property name: level
? Property type: number
? Is it required?: No
? Default value [leave blank for none]: 1
```
* The first property is `id`. It's like the prime key in relational database. We don't need user to specify `id` and we will auto generate `id` for them.
* The second property is `name`. That is the only thing we need user to specify.
* All of other properties like `level`, `attack` and `defence` are default. We will not nee user to specify.
We can create `weapon`, `armor`, and `skill` models in the same way.

Then if you go to `/src/models`, you will see `character.model.ts`, `weapon.model.ts`, `armor.model.ts`, and `skill.model.ts`.
Now open `character.model.ts` with your favourite editer, we are going handle the models relationship.

The language we use in LB4 is Typescript. It's very similar to Javascript. Don't worry if you did't use it before. LB4 already generate the most of code for you. We only need to add or edit few lines of code as needed.

If you take a look at `character.model.ts`, you will find it's very easy to understand.
```
  @property({
    type: 'number',
    id: true,
  })
  id?: number;
```
This means the type of `id` property is `number`, and it is a `id property`, like the prime key in relational database.

```
  @property({
    type: 'string',
    required: true,
  })
  name: string;
```
This means the type of `name` property is `string`, and it is required.
You can easily add or edit properties.

Now, we want to add relationships for `character` to indicate a `character` may has one `weapon`, `armor`, and `skill`.