Download Link: https://assignmentchef.com/product/solved-cs2030-lab-5-immutable-map
<br>






<h1>Task Content</h1>

<table width="680">

 <tbody>

  <tr>

   <td width="680"><strong>Immutable Map Topic Coverage</strong>Generic classMap and LinkedHashMap<strong>Problem Description</strong><strong>Map</strong>Map&lt;K,V&gt; is a generic interface from the Java Collection Framework, the implementation of which is useful for storing a collection of items and retrieving an item. It maintains a map (aka dictionary) between keys (of type K) and values (of type V). The two core methods are (i) put, which stores a (key, value) pair into the map, and (ii) get, which returns the value associated with a given key if the key is found or returns null otherwise.The following examples show how the Map&lt;K,V&gt; interface and its implementation LinkedHashMap&lt;K,V&gt; can be used. We use a LinkedHashMap in order to preserve the order of insertions into the Map.

    <table width="607">

     <tbody>

      <tr>

       <td width="607">jshell&gt; Map&lt;String,Integer&gt; map = new LinkedHashMap&lt;String,Integer&gt;(); jshell&gt; map.put(“one”, 1);$.. ==&gt; nulljshell&gt; map.put(“two”, 2);$.. ==&gt; nulljshell&gt; map.put(“three”, 3);$.. ==&gt; nulljshell&gt; map.get(“one”)$.. ==&gt; 1jshell&gt; map.get(“four”)$.. ==&gt; nulljshell&gt; map.entrySet()$.. ==&gt; [one=1, two=2, three=3]jshell&gt; for (Map.Entry&lt;String,Integer&gt; e: map.entrySet()) {…&gt;    System.out.println(e.getKey() + “:” + e.getValue());…&gt; } one:1 two:2 three:3</td>

      </tr>

     </tbody>

    </table>As expected, the Map implementation in the Java Collection Framework is mutable. You will also notice that unlike List or Set, Map does not implement Iterable, and hence one cannot perform the usual looping by using the enhanced for loop as shown below.</td>

  </tr>

 </tbody>

</table>

<table width="606">

 <tbody>

  <tr>

   <td width="606">for (Map&lt;String, Integer&gt; e : map) {System.out.println(e); }</td>

  </tr>

 </tbody>

</table>

<table width="606">

 <tbody>

  <tr>

   <td width="606">jshell&gt; ImmutableMap&lt;String, Integer&gt; map = new ImmutableMap&lt;String, Integer&gt;() map ==&gt; {} jshell&gt; map.put(“one”, 1)$.. ==&gt; {one=1} jshell&gt; map.put(“one”, 1).isEmpty()$.. ==&gt; false jshell&gt; map.put(“one”, 1).put(“two”, 2)$.. ==&gt; {one=1, two=2} jshell&gt; map.put(“one”, 1).put(“two”, 2).put(“three”, 3)$.. ==&gt; {one=1, two=2, three=3} jshell&gt; map map ==&gt; {} jshell&gt; map.isEmpty();$.. ==&gt; true jshell&gt; map.put(“one”, 1).put(“two”, 2).put(“three”, 3).put(“one”, 11) $.. ==&gt; {one=11, two=2, three=3}</td>

  </tr>

 </tbody>

</table>

Instead, the entrySet method is called to return a “view” of the map in terms of a Set, and since Set is Iterable, we loop the elements of the set instead.

<h1>Task</h1>

In this task you will build your own immutable version of Map that implements various methods to manipulate this map, by wrapping an immutable API over a mutable Map object. This pattern is sometimes known as the immutable delegation class which is simple to implement, although it comes with a performance overhead.

You are encouraged to familiarize yourself with the Map interface as specified by the Java API, as well as the ImList class that implements an immutable version of List.

<h1>Level 1</h1>

Create a class ImmutableMap with the type parameters K and V that “wraps around” an internal java.util.Map object.

Write a constructor ImmutableMap() that creates an empty map and define a toString() method that simple returns the string “{}” for an empty map. You should delegate implementation of toString to the internal Map.

jshell&gt; new ImmutableMap&lt;String, Integer&gt;()

$.. ==&gt; {}




jshell&gt; new ImmutableMap&lt;String, Integer&gt;().toString() $.. ==&gt; “{}”

<h1>Level 2</h1>

Include the method put that takes as argument the key of type K followed by the value of type V, and adds the keyvalue association into the map. You should delegate the implementation of put to the internal map’s put method. It is worth noting that the put method of java.util.Map returns the the previous value associated with key, or null if there was no mapping for the key. In our implementation, however, we shall just return the resulting map with the new mapping.

Include a isEmpty() method that outputs true if the map is empty, or false otherwise.

Moreover, if you have delegated the toString() method in the earlier level, then there is no need to modify the method further.

<h1>Level 3</h1>

<table width="606">

 <tbody>

  <tr>

   <td width="606">jshell&gt; ImmutableMap&lt;String, Integer&gt; map = new ImmutableMap&lt;String, Integer&gt;() map ==&gt; {} jshell&gt; map = map.put(“one”, 1).put(“two”, 2).put(“three”, 3) map ==&gt; {one=1, two=2, three=3} jshell&gt; map.get(“one”)$.. ==&gt; Optional[1] jshell&gt; map.get(“four”)$.. ==&gt; Optional.empty jshell&gt; map.get(1) $.. ==&gt; Optional.empty jshell&gt; map.keySet() $.. ==&gt; [one, two, three] jshell&gt; map.values()$.. ==&gt; [1, 2, 3] jshell&gt; map.entrySet()$.. ==&gt; [one=1, two=2, three=3]</td>

  </tr>

 </tbody>

</table>

<table width="606">

 <tbody>

  <tr>

   <td width="606">jshell&gt; ImmutableMap&lt;String, Integer&gt; map = new ImmutableMap&lt;String, Integer&gt;() map ==&gt; {} jshell&gt; map = map.put(“one”, 1).put(“two”, 2).put(“three”, 3) map ==&gt; {one=1, two=2, three=3} jshell&gt; // iteration via enhanced for loop jshell&gt; for (Map.Entry&lt;String, Integer&gt; e : map) {…&gt;     System.out.println(e);…&gt;     System.out.println(e.getValue() + ” is mapped to ” + e.getKey());…&gt; } one=11        is mapped to one two=22        is mapped to two three=33        is mapped to three jshell&gt; // old-style iteration jshell&gt; Iterator&lt;Map.Entry&lt;String, Integer&gt;&gt; iter = map.iterator() iter ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="9bf1faedfab5eeeff2f7b5d7f2f5f0feffd3fae8f3d6faebbfd7f2f5f0feffdef5efe9e2d2effee9faeff4e9dba3f9ffaaf9adfa">[email protected]</a> jshell&gt; while (iter.hasNext()) {…&gt;     System.out.println(iter.next());…&gt; } one=1 two=2 three=3</td>

  </tr>

 </tbody>

</table>

The get method in java.util.Map is specified to take in an Object, and returns the value to which the specified key is mapped, or null if the map contains no mapping for the key. In our implementation, we shall let java.util.Optional deal with the null value. Specifically our get method should now return Optional.empty is there is no mapping, or the value wrapped within an Optional otherwise. It is now a good time to study the specifications of the Optional class in the Java API.

Implement additional methods to obtain the keys, values, and key-value associations in the map. Study the keySet, values and entrySet methods of java.util.Map in the Java API.

<h1>Level 4</h1>

We would like to make our ImmutableMap iterable by iterating through the key‐value associations. Study how we made ImList iterable and modify ImmutableMap accordingly.

<h1>Level 5</h1>

Let’s write an application for our immutable map. We shall start by writing the Assessment class that implements the following Keyable interface.




<table width="681">

 <tbody>

  <tr>

   <td rowspan="2" width="37"> </td>

   <td width="607">interface Keyable {     String getKey();}</td>

   <td rowspan="2" width="37"> eee</td>

  </tr>

  <tr>

   <td width="607">Include a getGrade method that returns the grade of an assessment.

    <table width="606">

     <tbody>

      <tr>

       <td width="606">jshell&gt; new Assessment(“Lab1”, “B”)$.. ==&gt; {Lab1: B} jshell&gt; new Assessment(“Lab1”, “B”) instanceof Keyable$.. ==&gt; true jshell&gt; new Assessment(“Lab1”, “B”).getGrade()$.. ==&gt; “B” jshell&gt; new Assessment(“Lab1”, “B”).getKey() $.. ==&gt; “Lab1”</td>

      </tr>

     </tbody>

    </table>Next, write the Module class to store (via the put method) the assessments of a module in an immutable map for easy retrieval as part of answering queries. A module can have zero or more assessments, with each assessment having a title as a key — a unique identifier.

    <table width="606">

     <tbody>

      <tr>

       <td width="606">jshell&gt; new Module(“CS2040”)$.. ==&gt; CS2040: {} jshell&gt; new Module(“CS2040”) instanceof Keyable$.. ==&gt; true jshell&gt; new Module(“CS2040”).getKey();$.. ==&gt; “CS2040” jshell&gt; new Module(“CS2040”).put(new Assessment(“Lab1”, “B”))$.. ==&gt; CS2040: {{Lab1: B}} jshell&gt; new Module(“CS2040”).put(new Assessment(“Lab1”, “B”)).put(new Assessment(“Lab2″,”A+”))$.. ==&gt; CS2040: {{Lab1: B}, {Lab2: A+}} jshell&gt; new Module(“CS2040”).put(new Assessment(“Lab1”, “B”)).put(new Assessment(“Lab2″,”A+”)).g$.. ==&gt; Optional[{Lab1: B}] jshell&gt; new Module(“CS2040”).put(new Assessment(“Lab1”, “B”)).put(new Assessment(“Lab2″,”A+”)).g$.. ==&gt; Optional[{Lab2: A+}] jshell&gt; new Module(“CS2040”).put(new Assessment(“Lab1”, “B”)).put(new Assessment(“Lab2″,”A+”)).g $.. ==&gt; Optional.empty</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td colspan="3" width="681"></td>

  </tr>

 </tbody>

</table>


