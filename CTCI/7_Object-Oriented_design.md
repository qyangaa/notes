# 7 Object-Oriented Design

137

### Steps

+ Handle ambiguity: ask "6 W's" about application scenario:  who, what, where, when, how, why.
+ Define core objects
+ Analyze relationships: ask for clarification
+ Investigate actions: methods 

### Design Patterns

#### Singleton Class

A class with only one instance

```java
public class Restaurant{
    private static Restaurant _instance = null;
    protexted Restaurant(){...}
    public static Restaurant getInstance(){
        if(_instance == null){
            _instance = new Restaurant();
        }
        return instance;
    }
}
```

#### Factory Method

Interface of creating an instance of a class. 

+ Options for creator class:
  + Abstract
  + Concrete, provides implementation for Factory method. 

```java
public class CardGame{
    public static CardGame createCardGame(GameType type){
        if(type==GameType.Poker){
            return new PockerGame();
        }else if(type==GameType.BlackJack){
            return new BlackJackGame();
        }
        return null;
    }
}
```



## Interview Questions

### 7.2 Call Center

Imagine you have a call center with three levels of employees: respondent, manager, and director. An incoming telephone call must be first allocated to a respondent who is free. If the respondent can't handle the call, he or she must escalate the call to a manager. If the manager is not free or not able to handle it, then the call should be escalated to a director. Design the classes and data structures for this problem. Implement a method dispatchCall() which assigns a call to
the first available employee.

+ Handle ambiguity: 
  + Who: respondent, manager, director (multiple instances each)
  + What: dispatchCall()
  + Actions: takeCall(); escalate(); 
+ Define core objects
  + Caller
  + Call
  + abstract Employee
    + Director
    + Manager
    + Respondent
  + CallHandler
+ Analyze relationships: 
  + Caller initiates call
  + Call goes into CallHandler
  + CallHandler checks queue and availability of respondents
  + CallHandler dispatchCall() to proper employee
+ Investigate actions: 

```java
public class CallHandler{
    private final int LEVELS = 3;
    private final int NUM_RESPONDENTS = 10;
    private final int NUM_MANAGERS = 4;
    private final int NUM_DIRECTORS = 2;
    
    List<List<Employee>> employeeLevels;
    List<List<Call>> callQueues;
    
    public CallHandler(){...}
    
    public Employee getHandlerForCall(Call call){...}
    public void dispatchCall(Caller caller){
        Call call = new Call(caller);
        dispatchCall(call)
    }
    public void dispatchCall(Call call){
        Employee emp = getHandlerForCall(call);
        if(emp!=null){
            emp.receiveCall(call);
            call.setHandler(emp);
        }else{
            call.reply("Please wait");
            callQueues.get(call.getRank().getValue()).add(call);
        }
    }
    public boolean assignCall(Employee emp){...}
}

public class Call{
    private Rank rank;
    private Caller caller;
    private Employee handler;
    public Call(Caller c){
        rank = Rank.Responder;
        caller = c;
    }
    public void setHandler(Employee e){ handler = e;}
    public void reply(String message){...}
    public Rank getRank(){return rank;}
    public void setRank(Rank r){rank = r;}
    public Rank incrementRank(){...}
    public void disconnect(){...}
}

abstract class Employee{
    private Call currentCall = null;
    protected Rank rank;
    public Employee(CallHandler handler){...}
    public void receiveCall(Call call){...}
    public void callCompleted(){...}
    public void ascalateAndReassign(){...}
    public boolean assignNewCall(){...}
    public boolean isFree(){ return currentCall==null;}
    public Rank getRank(){return rank;}
}

class Director extends Employee{
    public Director(){rank = Rank.Director};
}
class Manager extends Employss{
    public Manager(){rank = Rank.Manager};
}
class Respondent extends Employss{
    public Respondent(){rank = Rank.Responder;}
}
public enum Rank{
    Respondent (0), Manager (1), Director (2);
    private int rank;
    private Rank(int r){rank = r;}
}
```



### 7.3 Jukebox

+ Handle ambiguity: 
  + What is Jukebox: A machine that selects CD and plays song.
+ Define core objects/ relationships
  + Jukebox
    + Of CDs
    + With Display
    + Controls playlist
  + CD
    + Has Songs
  + Song
    + Has Artist
  + Playlist (of songs)
  + Display
+ Actions:
  + Jukebox
    + selectCD
    + selectSong
  + CD
    + Has Songs
  + Song
    + Has Artist
  + Playlist (of songs)
    + Add, delete, shuffle
  + Display
    + show CD
    + show Song
      + Show artist
    + show next

```java
public class Jukebox{
    private CDPlayer cdPlayer;
    private User user;
    private Set<CD> cdCollection;
    private SongSelector ts;
    
    public Jukebox(CDPlayer cdPlayer, User user, Set<CD> cdCollection, SongSelector ts){...}
    public Song getCurrentSong(){return ts.getCurrentSong();}
    public void setUser(User u){this.user = u;}
}

public class CDPlayer{
    private Playlist p;
    private CD c;
    
    public CDPlayer(CD c, Playlist p){...}
    public CDPlayer(Playlist p){this.p = p;}
    public CDPlayer(CD c){this.c = c;}
    
    public void playSong(Song s){...}
    public Playlist getPlaylist(){return p;}
    public void setPlaylist(Playlist p){this.p=p;}
    public CD getCD(){return c;}
    public void setCD(CD c){this.c = c;}
}

public class Playlist{
    private Song song;
    private Queue<Song> queue;
    public Playlist(Song song, Queue<Song> queue){...}
    public Song getNext(){return queue.peek();}
    public void addSong(Song s){queue.add(s);}
}
public class CD{}
public class Song{}
public class User{}
```

### 7.4 Parking Lot

+ Handle ambiguity: 
  + What kind of parking lot: multiple levels, compact spot or normal spot, motorcycle can part in any slot
+ Define core objects
  + abstract Vehicle
    + compact car
    + normal car
    + motorcycle
  + parking lot
  + levels
  + Parking spot
+ Analyze relationships: ask for clarification
  + abstract Vehicle: size
  + parking lot
    + levels
      + Parking spot: has car (or not), can fit car (or not)
+ Investigate actions: methods 
  + abstract Vehicle: size
  + parking lot
    + levels
      + Parking spot: addCar(), removeCar()

```java
public enum VehicleSize{MOTORCYCLE (0), COMPACT (1), NORMAL (2)..}

public abstract class Vehicle{
    protected String licensePlate;
    protected VehicleSize size;
}
public class CompactCar extendes Vehicle{
    public CompactCar(){size = VehicleSize.COMPACT;}
}
...
public class ParkingLot{
    private Level[] levels;
    public ParkingLot(){...}
    publoc boolean parkVehicle(Vehicle vehicle){...}
}
public class Level{
    private int floor;
    private ParkingSpot[] spots;
    ...
}
public class ParkingSpot{
    ..
}
```

### 7.5 Online Book Reader

+ Handle ambiguity:
  + A number of users. Number of books. One active user a time. One active book a time. 
  + read book: keep track of current pages
+ Define core objects
  + Database: Users, Books
  + Book:  author/ year/ abstract/ content
  + User: account info/ List of {book: current page}
  + Reader: 
    + authentication: login/ logout
    + book selection: selectbook(book)
    + reader control: nextPage(), previousPage(), saveProgress()

```java
public class Book{...}
public class User{
    name, email, ...
    private Map<Book, int> bookList;
    public setProgress(Book book, int page){...}
    public getBookList(){...}
    public getProgress(Book book){...}
}
public class Reader{
    user, book...
    public signIn(){..}
    public signOff(){...}
    public selectBook(book){...}
    public pageControl(...){...}
}
```

### 7.6 Jigsaw

 Implement an NxN jigsaw puzzle. Design the data structures and explain an algorithm to solve the puzzle. You can assume that you have a fitsWith method which, when passed two puzzle edges, returns true if the two edges belong together.

```java
public enum Orientation{
    LEFT, TOP, RIGHT, BOTTOM;
    public Orientation getOpposite(){
        switch (this){
            case LEFT: return RIGHT;
                ...
            default: return null;
        }
    }
}
public enum Shape{
    INNER, OUTER, FLAT;
    public Shape getOpposite(){...}
}
public class Piece{
    private HashMap<Orientation, Edge> edges = new HashMap<Orientation, Edge>();
    public Piece(Edge[] edgeList){...}
    public void rotateEdgesBy(int numberRotations){...}
    public boolean isCorner(){...}
    public boolean isBorder(){...}
}
public class Edge{
    private Shape shape;
    private Piece parentPiece;
    public Edge(Shape shape){..}
    public boolean fitsWith(Edge edge){...}
}
public Puzzle(int size, LinkedList<Piece> pieces){
    private void setEdgeInSolution(LinkedList<Piece> pieces, Edge edge, int row, int column, Orientation orientation){
        Piece piece = edge.getParentPiece();
        piece.setEdgeAsOrientation(edge, orientation);
        pieces.remove(piece);
        solution[row][column] = piece;
    }
    private boolean fitNextEdge(LinkedList<Piece) piecesToSearch, int row, int col);
    public boolean solve(){...}
}
```



### 7.7 Chat Server

+ UI
+ API
  + User authentication
  + ChatsAPI: receive, sent, update
+ Database
  + User: 
    + Name, email, passwords...
  + Chats:
    + participants
    + chat: sender, time, content

Hardest problems:

+ Dealing with bad requests when Internet connection is bad
+ Dealing with real-time data flow
+ Safety 



p317