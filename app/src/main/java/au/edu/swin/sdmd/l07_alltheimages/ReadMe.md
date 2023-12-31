L07 All the images
------------------
Complete sample code for Module 7:

# Overview

## 2023 Update
- use packages to organise the components 
- move initData to DataSource class
- Handle the user's update on Location's visited attribute
  in the detailed activity

To further explore lists, we will look at RecyclerView. This is a new list widget and is the default way now to build lists.

It was designed to overcome feature bloat in ListView and to enforce good practices. The long story (about 45 minutes) can be found at https://www.youtube.com/watch?v=LqBlYJTfLP4.

# The RecyclerView adapter

As we saw with the basic list example, we need an adapter to connect the view and the data.

We saw the the basic list view does not enforce use of patterns such as ViewHolder that would improve performance of lists.

RecyclerView does enforce this, and its adapter has some required methods as a result.
```
onCreateViewHolder(parent, viewType): ViewHolder
getItemCount: Int
onBindViewHolder(holder, position)
```

To give you an idea of the process, here's a diagram based on BNR Chapter 9.

If you have PlantUML installed, place your cursor inside the following text and click on the PlantUML button on the right-hand side of the screen.

```
@startuml
participant RecyclerView
participant Adapter
RecyclerView -> Adapter : onCreateViewHolder()
note left: I need a ViewHolder
Adapter -> RecyclerView : ViewHolder
note right: Ok. Inflate view, create new holder, pass it back.
RecyclerView -> Adapter : onBindViewHolder(ViewHolder, 0)
note left: I need data at index 0 to be shown in this ViewHolder
note right: Ok. Gets data at position 0, set values of views.
@enduml
```

# Setting it up

You'll note that we need a layout manager this time around -- we didn't need that for a ListView.

This needs to be set in the activity. We're using a LinearLayoutManager that shows a list; for photos or a grid you'd want a GridLayoutManager.

```
linearLayoutManager = LinearLayoutManager(this)
listView.layoutManager = linearLayoutManager
```

You'll also need a custom row layout, similar to the previous project.

# More fun stuff

At the moment we've set up a classic list. This is all very nice, however we'd probably like it to be more interactive.

Handling clicks can be done in a few different ways. One approach is to assign it in the onBindViewHolder function, as seen in https://antonioleiva.com/recyclerview-listener/.
However this is not the most elegant approach, and one needs to be wary of creating objects on every bind call.

The more flexible way is to attach the listener in the view holder. The ViewHolder will need to be declared as an inner class so it can access the listener passed from the activity.