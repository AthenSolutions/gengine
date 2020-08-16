## Gengine

**NOTICE:**\
*Gengine is currently under heavy development, and even more features will be coming in the near future*\
*Gengine is also currently a private library, requiring authentication to request from the maven repository.*\
*If you are a game developer wishing to use **Gengine**, contact Gleeming#0001 on discord.*
\
\
Welcome to [Gengine](http://github.com/GleemingKnight/gengine), a modern 2D game library supporting a multitude of features.\
\
Gengine was made by game developers for game developers, which means it has all the features you wish other frameworks had,
without the features you wish they didn't have. Gengine is made of pure java, not using any build libraries, which makes it extremely lightweight.\
\
**Setting up your project**\
\
*Maven*\
In order to use Gengine Maven, you must add our repository, then add the dependency in your **pom.xml**
```xml
<repository>
    <id>athen-repo</id>
    <url>http://athen.cc:8082/artifactory/libs-release/</url>
</repository>

<dependency>
    <groupId>me.gleeming.gengine</groupId>
    <artifactId>gengine</artifactId>
    <version>1.0-20200815.044625-1</version>
</dependency>
```
\
*Gradle*\
In order to use Gengine Gradle, you must add our repository, then add the dependency in your **build.gradle**
```gradle
repositories {
    maven {
        url "http://athen.cc:8082/artifactory/libs-release/"
    }
}

compile(group: 'me.gleeming.gengine', name: 'gengine', version: '1.0-20200815.044625-1')
```
\
\
**Creating the main class**\
The main class must extend ```Gengine2DGame```, within this class you will create your first screen.
```java
public class TestGame extends Gengine2DGame {
    public TestGame() {
        super("Test Game (1.0-SNAPSHOT)", width, height);
        /* There is also a different super method, supporting a window icon. */
        super("Test Game (1.0-SNAPSHOT)", new Resource("path-to-window-icon"), width, height);
        
        /*
            The next step when creating a game using gengine, is to define the first screen.
            In this tutorial, we will be using a MenuScreen.
        */
        Gengine.getInstance().changeScreen(new MainMenuScreen());
    }
}
```
\
**Creating the main menu screen**\
The main menu screen will be using a background image, along with buttons to start the game.
```java
public class MainMenuScreen extends MenuScreen {
    public MainMenuScreen() {
        super(new Resource("menu-back-ground-path"));
        /* 
            There is also a different super method, supporting an animated menu screen. 
            We will cover how to create animations later in the tutorial.
        */
        super(new Animation());

        /* 
            Time to create the menu buttons, buttons support having a highlighted
            version of themselves for whenever the mouse is over them.
        */
        addButton(new Button(new Resource("unselected-button-path"), new Resource("selected-button-path"), x, y, width, height).addListener(new Button.Listener() {
            public void buttonClick() {
                /*
                    This will be the play button, so we want to go to the main game
                    screen whenever the player clicks the button
                */
                Gengine.getInstance().changeScreen(new GameScreen());
            }
        }));
        /* You can also have a button without a selected image */
        addButton(new Button(new Resource("unselected-button-path"), x, y, width, height).addListener(new Button.Listener() {
            public void buttonClick() {
                /*
                    This will be the quit button, so we want to exit the application
                    with exit code '0' whenever the player clicks the button
                */
                System.exit(0);
            }
        }));
    }
}
```

**Creating the main game screen**\
When creating a screen, you have access to all the draw queues available.
```java
public class GameScreen() extends GengineScreen {
    /*
        This will be the game screen, where all the game objects 
        are drawn. You can add any draw queue type to the list:
        - FilledMathRectangle
        - FilledOval
        - FilledRectangle
        - HollowMathRectangle
        - HollowOval
        - HollowRectangle
        - Resource
        - Text
        
        There will eventually be more draw queues, although these are the starter ones.
    */    
    public List<DrawQueue> draw(List<DrawQueue> queued) {
        /* 
            Here's an example of how you can draw text to the screen,
            you can use custom fonts as shown in the second example
        */
        queued.add(new TextDrawQueue("Hello world", x, y, size).setColor(Color.ORANGE));
        queued.add(new TextDrawQueue("Hello world", x, y, font).setColor(Color.RED));
        
        return queued;
    }
}
```
\
**How to use animations**\
In order to use an animation, add all of the sprites to your resources and follow the tutorial\
Animations use a formula to ensure the correct resource is always being returned
```java
/* You can use any fps you want, and you can add how ever many images as you want. */
Animation animation = new Animation(fps, new Resource("player1.png"), new Resource("player2.png"), new Resource("player3.png"));

/* Whenever it comes time to draw the figure, you can use getCurrentFrame(); */
public List<DrawQueue> draw(List<DrawQueue> queued) {
    queued.add(new ResourceDrawQueue(animation.getCurrentFrame(), x, y, width, height));
    
    return queued;
}
```
\
**How to play sounds**\
In order to play sounds, you use the GengineSoundEngine, add your sounds to your resources and follow the tutorial
Currently gengine supports .WAV and .MP3 files, although we are hoping to expand to other file extensions
```java
/* You can add music to your game, that repeats on loop by using the following code */
GengineSound backgroundMusic = new MP3GengineSound("path-to-mp3-music");
GengineSoundEngine.getInstance().changeMainMusic(backgroundMusic);

/* You can play a sound effect in your game by using the following code */
GengineSound soundEffect = new WAVGengineSound("path-to-wav-sound");
GengineSoundEngine.getInstance().playSoundEffect(soundEffect);

/* You can also change the volume of a sound by using */
backgroundMusic.setVolume(0.25f);
soundEffect.setVolume(0.5f);
```
\
**How to use the camera**\
Gengine currently only supports one camera and one camera type for the sake of simplicity,
although in the future we will be adding features to have more cameras.
```java
GengineCamera camera = GengineCamera.getInstance();

/* You can change the cameras position by using the following code */
camera.changePosition(x, y);
```
\
**How to receive input**\
You can check if a key is pressed in gengine by following the tutorial below
```java
/* It will usually be a good idea to receive input in the draw method */
public List<DrawQueue> draw(List<DrawQueue> queued) {
    /*
        You can check if any key is being pressed by using the following code,
        all standard keyboard keys & 6 mouse buttons are currently supported.
    */
    if(GengineInput.getInstance().isPressed(GengineInput.Key.SPACE) {
        /* This is where you would execute the code */
    }
    
    /* You can use the same method to receive mouse input */
    if(GengineInput.getInstance().isPressed(GengineInput.Key.LEFT_CLICK) {
        /* This is where you would execute the code */
    }
    
    return queued;
}
```
\
**How to load fonts**\
Currently Gengine only supports the TrueType font extension, although we are hoping
to expand it to more font extensions in the near future
```java
Font font = new TTFGengineFont("path-to-font");

/* You can change the size of the font by using the following code */
font.setSize(size);

/* You can change the color of the font whenver you draw it */
public List<DrawQueue> draw(List<DrawQueue> queued) {
    queued.add(new TextDrawQueue("Hello world", x, y, font).setColor(Color.RED));
    
    return queued;
}
```
\
**How to load and edit resources**\
Gengine supports any type of image type that windows supports.
You can flip & rotate your image inside of gengine.
```java
Resource resource = new Resource("path-to-image");

/* To flip the image, use one of the following methods */
resource.flipVertically();
resource.flipHorizontal();

/* To rotate the image, use one of the following methods */
resource.rotateClockwise(degrees);
resource.rotateCounterClockwise(degrees);
resource.rotate(degrees);

/* You can also change the color of the resource on draw */
public List<DrawQueue> draw(List<DrawQueue> queued) {
    queued.add(new ResourceDrawQueue(resource, x, y, width, height).setColor(Color.RED));
    
    return queued;
}
```
\
**How to use the default gengine math methods**\
Defaulty, gengine comes with multiple math utils to assist you on collision and vector storage.
```java
/* 
    The following code would be how you check a collision between two rectangles,
    the gengine math classes are carefully thought out to be effective in every situation.
    
    Rectangles in particular, use equations to store every point on the polygon to ensure
    that rectangle collision is done correctly.
*/

Rectangle rect1 = new Rectangle(buttomLeftCornerX, bottomLeftCornerY, width, height);
Rectangle rect2 = new Rectangle(buttomLeftCornerX, bottomLeftCornerY, width, height);

if(rect1.colliding(rect2)) {
    /* 
        This would be how you check for the collision.
        There are lots of other gengine math methods & we will be adding even more.
    */
}
```
\
*These are all of the main features that gengine currently has*,\
*this page will be updated when more compact and better features are added to gengine.*
