State management i React.js med hjälp av React Query 
 
 
 

 

A degree project carried out by: 

Oliver Rodin 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 
 

 

 

1. Summary 

 

 

 

 

 

 
 

 

 

 
 

 

3 Introduction 
 

3.1. Background 
 

Today, the demand for scalable and efficient web applications (PWAs) is high. State management and data management is a critical factor in developing a robust and easy-to-maintain application, and it is especially important for frontend developers to have a deep understanding of that concept. 

During my Lia at Apptech 24 AB, I have exclusively worked in one of their products, EasyStaff. The app is a modern take on an intranet where companies share news, chat, invite to activities, keep track of staffing with absence management and calendar. All this in a safe and secure environment protected by BankID.  

During the work with Easy Staff, I have understood the importance of state management and data communication. Quite quickly I saw an opportunity to immerse myself in this in my degree project. Partly to build on my portfolio with relevant knowledge but also to generate benefit over Apptech. 

After careful consideration together with my supervisor, it has resulted in a migration of Easy Staff's current data warehouse (state management, data-fetching) consisting of various technologies and libraries to a solution based on React Query.  

Since React Query has been in the pipeline at Apptech for a while, my thesis is an excellent test opportunity for the library to be able to evaluate afterwards if it is something that should be implemented in future React projects at Apptech. 

 

3.2 Purpose  
 
Basically, the purpose is to test React Query as a possible solution to a recurring problem in today's component-based front-end development, state-management. 

In addition, the purpose is also to deepen my own knowledge of React Query and state management in general. Which will hopefully benefit me in my future career. 

By bringing the library into an already existing application, I feel that my purpose is in focus, as I do not have to think about developing a foundation in the form of a database, a backend and a UI, but I can focus exclusively on the depth of React Query and its possibilities to centralize data management and communication with the backend. 

 

 
 

3.3. Delimitation 
 

I will not develop my own application from scratch. 
 

I will not further develop Easy Staff functionally, but I will focus on implementing and evaluating React Query in the already existing functionality. 
 

I will use Easy Staff's existing API and not change any shape or structure in it. I want to see if React Query can adapt to the already existing needs.  
 

I will use React Query to manage and centralize data, and will not investigate other possible solutions or state-management libraries.  
 

I may have to change the scope of the implementation during the work due to lack of time. 

 

3.4 Methods 

To achieve the purpose of the thesis and test React Query as a possible solution to state-management in Easy Staff, I have chosen to follow an iterative work process. 

First, I have conducted a thorough research of React Query, its benefits and limitations, and how it can be applied in Easy Staff. After the research, React Query was implemented in Easy Staff and advantages and disadvantages appeared. 

After implementation, I have tested and evaluated the results by measuring performance, user experience and developer experience. I have also conducted a comparison with Easy Staff's previous state-management solutions to evaluate how React Query performs in comparison to the already existing solution. 

 

 
 

4 Implementation  

 

4.1 Introduction to React Query and its functionality 
 

React Query is a powerful opensource library created by Tanner Linsley in 2019.   
It handles retrieval, caching, synchronization, and updating server states. 

Overall, the library gives the user the opportunity to build fast, responsive applications with improved UX. This by reducing the number of network requests, simplifying data management, improving performance and user experience. The library offers many features that, despite their complexity, are adaptable to a variety of uses. 

 

4.1.1 Communication 
 

React Query communicates between frontend and backend, it can handle a variety of data sources such as REST API, GraphQL and websockets. In this report, however, I will start from the first option using axios.  
Communication takes place regardless of data source through React Query's built-in hooks, below are two examples of what it can look like. The first example retrieves data from source, and the second sends data to source. Common to both is that they are encapsulated in a custom-hook so that you can share the logic among several components in the application and thus do not have to repeat code:  
 
 

 

 
 

 

 

When the application uses an RQ hook to retrieve data, a unique "query-key" is specified for each unique data request. This query-key is then used to identify the data request and to cache the data for further use. 

When a data request is made, it is returned immediately without sending the request if it should already be in the cache. If the data is not in the cache, the network request is sent to the API to retrieve it from the database and once the download is complete, it is automatically cached by RQ under the specified key.  

If you combine RQ's technology with your own custom hooks as in the examples above, the same data can easily be retrieved by several different components in React as long as you specify the correct query-key in the query. This minimizes the number of network requests and significantly improves performance. 

 

4.1.2 Cache 
 

React Query's caching system has been mentioned a bit in the previous paragraph, but here it is described in a little more detail. The cache works in such a way that it stores data in memory for a limited time, by default the data is stored for 5 minutes but the caheTim can be set on each useQuery individually if desired. Of course, this makes it extremely fast for the application to render data on the screen that has been previously retrieved without the need for new network requests. 

In addition to this, it is possible to configure the cache in several ways, some examples are cachTime mentioned above, ie how long should the data stay in the cache, you can set a specific storage type e.g. "localStorage" or "sessionStorage" and decide what should happen when the cache becomes full.  

 
 

React Query has an associated library called React-Query-DevTools that provides a fantastic overview of what data is cached under which keys, if they are mounted, and what status each cache key has. To simplify working with RQ and understand the full potential of the library, devtools should be used, it is every RQ developer's best friend and a fantastic compliment from the RQ team.  

 

4.1.3 UX improvements 

 
In addition to retrieving, caching, synchronizing, and updating server states, React Query can also improve the user experience (UX) in other ways. For example, RQ automatically handles load state and error handling, making it easier to build user-friendly interfaces that can provide users with important feedback while retrieving data from the database.  

RQ also offers features such as:  
 - Polling - using RQ's refetch function, retrieval of data can be performed at a predetermined time interval. 
 - Pagination/Infinite Scroll – Rendering paginated data occurs very often today, this simplifies RQ with just a few simple changes to the useQuery hook. -  
Optimistic Updates - Update Cache before server, UI can be rerendered instantly. 
 

These and many other not yet mentioned features facilitate the development of complex applications based on real-time updates. 

Overall, RQ drastically simplifies the management of asynchronous server data in a way that not many other state management libraries do. By centralizing server data in one and the same cache but under our own unique query-keys, we have access to the same data from the entire application and if we use RQ's hooks in our own custom hooks, equivalent code does not need to be repeated, but it is just a matter of calling the right hook to get access to the right data.   

This means that developers can focus more on building the user experience and less on handling data and network requests as much of the logic for this takes place under the hood of RQ's hooks. This can lead to shorter development times and more robust applications that over time are easier to maintain. 

 

 

 
 

4.2 Implementering av React Query i Easy Staff 

Easy Staff is a modern intranet for communication in the workplace. The application consists of 8 modules: 

Chat 

News 

Career  

Development 

Employees 

Lunch 

Admin/Profile 

Calendar 

In addition to these, there are also some features such as login/logout, notifications and authentication. It was in the order above that I implemented the library in the application, module by module. 

My goal after the first week of in-depth React Query and review of Easy Staff was immediately that I would implement the library throughout the application. Since the work in the different modules has been similar, I will describe the approach using the chat module below. 

Week 2 I started the implementation by installing the library in the application and getting the extremely small part with boilerplate code in place to be able to start using the library. 
  

 

 
 

This is the boilerplate code that must be in place to start using RQ. In addition to the boilerplate code, I also set a foundation for how I wanted the RQ code structured in the document tree. I ended up creating a "hooks" folder in each module and divided the logic on a module plane, RQ logic for the news module had to live in that module folder.  In this way, I think you got a good overview of the whole thing. 

 

4.2.1 Retrieving data 
 

After installation, it was just a matter of replacing the communication with the backend and basing it on RQ hooks that were encapsulated in their own custom hooks as in the examples at the beginning. I chose to set up the logic in such a way to easily be able to divide it between components in the application. Below we see an example from my code, where I use RQ's useInfiniteQuery hook that is available to easily retrieve pieces of data that are then filled in as the screen scrolls down.  

 
 

 
 

 

That approach also complements the RQ cache in the best way. If I want to retrieve my states in this case my chat list from the cache, I can easily do so using my custom hooks and this applies throughout the application.  
Below I download my chats to the ChatListPage component and then render them in the UI:  

 

 
 

 

4.2.2 Updating data 
 

Once I felt I had a grasp of how to retrieve data from the server, it was time to attack updating the data. Here too, this is done using custom-hooks, but here I have instead encapsulated RQ's useMutation hook to update/mutate the data. Unlike when retrieving data, you often want to perform various after-actions when you update the data. This is solved with different callbacks in the useMutation hook, if the data is uploaded correctly, you can with the help of onSucces redownload the list of chats in this case to render out the new chat you just created. But if something should go wrong and useMutation would throw an error, there is the possibility to pick this up in an onError callback where you can notify the user that something has gone wrong and console-log the error message on behalf of the developers. Here you can see an example of a custom hook that uploads a new chat to the database.   

 

 

 

But if you hide the useMutation hook in a custom hook, how can you perform different actions from specific components? This is usually where UI and client-states are, do you need to pass these as arguments to the custom-hook? Answer no, here RQ shines. Unlike retrieving data and calling a useQuery that runs directly automatically, it is now the developer who decides when to execute the useMutation hook. If you have a component that has the function to update data, you call the specific custom-hook from the component and when it is time to run the update, you call a function called mutate(), it is that function that decides when the useMutation hook should run and in turn the data should be sent to the server.  

 
 

 

It is also in the mutate function that you can access the same callbacks as in the hook, onSucces, onError and onSettled. This means that you can set specific actions for each component that uses the same custom-hook while in the custom-hook you can run actions that you always want to run regardless of where the hook is called from. Examples of component-specific actions could be closing a modal, navigating to a new route after success, or notifying the user that something has gone wrong if the update throws an error. Here you see an example from my code where the mutate function is called in a submitHandler which in turn uploads the new chat to the database. 

 

 
 

 
 
4.2.3 State management  

After implementing React Query throughout the chat module, the number of local states in the react components has been drastically reduced, parts of them have been centralized to RQ's cache and thus they are now retrieved via the respective custom-hook. Loading- and Error-states are now handled in RQ's logic, which means that they are now also collected from the same place.  

This may not look like a big difference but the code on the right is after RQ is implemented, the component now only has states that are directly linked to the UI. All mutating of data takes place directly in the cache.  

The main difference, however, is the large reduction in temporary states, ones that have existed to keep track of a Boolean value for a toggle or active object of some type. These have been completely removed and are now managed directly in the cache. Above are isPinned and isPrivate examples of such, before RQ these needed to exist to set the value of two toggles, partly the value showed the temporary state on each toggle before the component was rerendered after a submit. Then it was also these states that were included in the submit.  

 
 

 

With RQ, the two toggle buttons instead pick the value directly from the cache and when either of them is changed, the cache is also updated directly using "optimistic updates", which means that the value in the cache is updated directly on clicks and the UI is rendered just as quickly to the new value. After that, the data is sent to the database in the background, if the refresh returns success, the cache performs a background retrieval of the data and is updated and given the status fresh. However, since the new value is already in the cache, this is not something that is visible in the UI. Should it be the case that the update should throw an error, a rollback is performed and the toggle is rerendered. Below you can see what this looks like in code in the useMutation hook:  

 

 
 

 

4.2.4 Improve UX 
 

With React Query, I have been able to improve the user experience on several levels. Several improvements take place under the hood of RQ, if a route has been visited by the user, its data is in the cache so the user goes back to the same route later, the route is rendered immediately with the data from the cache. If the user logs in to Easy Staff to check today's lunch and then works for a while on other things, the next time they go back to the Easy staff tab in the browser, the page is redownloaded in the background but only rendered if new data exists. The same thing happens after a temporary network dip. This is done to ensure that the user receives the latest information. 

In addition to what happens under the hood, the developer has many opportunities to improve the user experience, such an improvement is as described in the paragraph above, the ability to make "optimistic updates" directly in the cache.  

Another possibility is RQ's ability to retrieve data before rendering the UI through prefetch(). This has made it possible to remove large amounts of solder-bars and spinners. Which has resulted in a rapper UI that feels more pleasing to the user's eye. An example of a prefetch in the chat module is that when I hover over the chat icon before clicking on it, the chat list and pinned chats are prefetched, which means that the data already exists when the user clicks on the icon and the chat modal opens. 

 

The pretech function populates the cache in the same way as a fetch with useQuery, so if they share the same queryKey, they populate the same cache entry. The code for the previously mentioned prefetch looks like this: 

 

 
 

 
The developer can also put a refetch interval on. selected query. I have used this in the Chat module to get a smoother flow in the chat. As soon as a chat room is open, the messages are refetched every two seconds. In this way, the chat is experienced directly and raptly. The only thing that needs to be done to take advantage of this feature is to put a refetch range in the selected useQuery hook:  

 

 

The approach I described above using the chat module, retrieving data, updating data, state management and improving UX I have used through all modules in the application. Thanks to the fact that I started with one of the most complicated modules, the job after the flow has been good, my ability to see what react query can and cannot add has improved module by module and the end result of the implementation I am satisfied with even though I have chosen to leave out some smooth functions due to some external factors.  

 
 

4.3 Pre- and post-implementation code and performance comparison 

4.3.1 Difference code and developer experience 

After implementing React Query, the developer experience has noticeably improved. Thanks to the cache, the application now has a centralized location for server states. Communication between components and states takes place through custom-hooks, which also facilitates the use of the data enormously. Components that need to access the same data can now do so in a simple way as it is just to call the current hook and from there pick the data to be rendered, which in turn also reduces the number of calls to the database.  

Before implementation, each component made its own API calls for all the data needed, the component needed data from three endpoints so it resulted in three useCallbacks and useEffects() respectively. As the application grows, the components become increasingly complex and eventually almost impossible to maintain.  

At this time, three custom-hooks are called instead. By working in this way, much of the logic is removed from the react components and divided into several custom-hooks that together form a service layer for all logic. This means that the components become more pure view components that handle the rendering of the UI. In the end, the amount of code becomes equivalent, but it is spread out and ends up in more logical compartments. Repot becomes more transparent and the application more scalable. If you want to replace React Query with some other library to handle state-management and data retrieval in the future, you do not need to go into the components and cave. Either you modify the service layer that already exists or you just create a new one that provides the components with data of the same structure. Future-proof code is good code. 

Link to component samples with and without React. Query. 
 

4.3.2. Comparison performance 

When it comes to performance, the comparison has been more challenging, performance measurements can be complicated and are dependent on several factors such as hardware, your browser and other applications running on the computer. To get accurate measurements, the application should be tested on different devices and in different browsers. The time for this has not really existed during these weeks, the focus has also automatically ended up on the user experience as that is where React Query can work wonders. With so much pre-configured under the hood of RQ, it's also hard to know what and how to test the performance.  

What has been done in the test path is lighthouse reports on the code before and after implementation to get a concrete receipt of any performance improvements. In the reports, the performance difference has not been particularly noticeable, partly because the application suffers from other major problems such as the handling of images and a too large DOM, so that's where lighthouse has put its focus.  

What can be deduced concretely from the reports is that after the implementation of React Query, performance has increased from 69 to 82 where it is "Total Blocking Time" that has primarily improved, from 310ms down to 130ms. This means that the time until the user can start integrating with the app has become shorter and it sounds like a very reasonable performance improvement because that is exactly what React Query improves with its cache.  
Both lighthouse reports will be included as attachments if there is an interest in reading them.  

 
 

In addition to lighthouse, I have also measured the page reload time on 5 of the routes in the application where you can see a consistent improvement in performance. On all routes, the reloading time has been reduced by one second. See results below: 

Nyhetssidan: 
 
Kalendersidan: 
 
Utvecklingsidan:  
 

 
 

Personalsidan: 
 

Kärriärssidan:  
 

After the comparisons that have been made, it is only to be stated that performance optimizations are not just about numbers, but the user experience has at least as big an impact on results. It is for the end user the application is built and the main goal is to satisfy them.  

 
 
 

4.4 Evaluation 

In broad terms, I have done what I had planned in my degree project, I have immersed myself in React Query, and built on with a lot of knowledge about state-management in general. I have managed to implement the library and my new knowledge in an already existing application, in this case Easy Staff. My goal was to centralize the entire application's server states using React Query, I have largely succeeded with that, apart from one module (notifications) that I chose to skip but it was already included in the plan in the planning stage.  

If I compare the work done with the planning, I have managed to estimate quite well, the only things that have changed a bit on execution versus planning have been illnesses and VAB at home. This has meant that I have had to work a little longer shifts, but if you knock out the hours, I have probably spent about a week on in-depth study in React Query and then be able to manage the implementation of the library during weeks two and three respectively and then be able to complete the report during week four. 

If I read through my purpose with this degree project, I think it has been fulfilled as best it can in four weeks. I have definitely increased my understanding and highlighted the importance of state management in React.js. During the implementation of the library, I have not only learned how states are handled in it, but also the importance of gathering these in one place to increase the scalability of the application. I have also learned the difference between client and server states, previously I had not even reflected on the fact that it was possible to group states in this way and that there are actually differences in handling them. Client-states are owned by the application, all knowledge of the state lives in the app while server-states are owned by the server, the client borrows the latest data to render it on the screen. This means that we have to be more affirmative when it comes to managing server states, we have to constantly make sure that we render the latest data. Since the state's truth is on the server, this must be communicated via network requests and this is what React Query solves so well with many of their functions. It ensures that you always have fresh data without overproducing network requests, and the requests made often happen in the background and do not affect the end user.  

Have I reached my predefined goal with the degree project?   
I think I have, there are several concrete examples of how React Query can streamline Easy Staff, both in terms of performance but also purely in terms of code. Many of these examples I have mentioned and demonstrated in this report. 

 

 
 

4.5. Analysis 

 

I have gotten a lot done during this degree project but there are also things that I would have liked to do that have fallen away for various reasons. The foundation is there, but it is mainly functions in React Query that do not match with the application's already existing architecture. 

The biggest dropout and the only thing I miss has been the use of "optimistic updates" which is actually one of React Query's absolute biggest "selling points", many are moving towards a use of RQ thanks to this so it is sad that it has not worked so well for me. The reason for this is partly the structure of Easy Staff's API but also the application's architecture, which does not always fit with the requirements to be able to perform an "optimistic update".  

The function is based on pure data bindings, if you update a property in an object, the entire object is updated based on ID, in this way you can directly retrieve the cached data with the same ID and update it before sending it to the server. In my case, the API is sometimes structured in such a way that an update of a property can have its own put function and thus its own network call. This means that only the property's value and the object's ID are passed in the request-body, which means that you do not have the entire object when performing the cache update.  
But then you can spontaneously think, surely it's just retrieve the cached data with the same id, spread it and replace the right property with the new value?  
 Theoretically, it absolutely works, but then it is based on having a static queryKey because it is through it that you access the right cache entry in the cache and can retrieve and put the data you want to update. Well it should work, you can think, no oddities.  

In many of my cases, this does not work either as I use useInfiniteQuerys with dynamic queryKeys, that is, it is possible to filter the data with a search string or a selected category. 

 

As a result, I can't retrieve the active data from the cache because I can never be sure what my queryKey looks like and how am I supposed to refresh the cache?  

A solution to the problem could be that I retrieve the current object to be updated using a useQuery and a getById, then I have a static queryKey: 

Then in my optimistic update I can just download that object and then update it and retune back to the cache. It absolutely works, the object is updated in the cache during the above cache entry but the update is not rendered until after the news page has been rerendered. Now what this? I didn't understand anything in the moment I sat and tried my hand at it, but after a while it became clear to me. 

 
 

The rerendering of the news item does not happen because it is not the getById data that is rendered on the screen basically, but the data that is visible on the screen comes from the list of news items. 

But what if, in my update, I retrieve the list from the cache and then spread the list and replace the object with the same ID with my getById object? This also doesn't work because I can't retrieve the list from the cache because I still can't know what the list's queryKey looks like. 

This is where the list of different approaches is coming to an end, but I have one last idea.   
If when I retrieve the object with getById I re-render the selected news with that data, should the optimistic update work well? Yes, that's right, but the way the news list is built, a modal does not open when you select a news item, but the news card just gets bigger with the help of an animation, which means that when getByIdn is executed, the entire news list will be rerendered and that is something we absolutely do not want as it results in an extremely poor user experience.  

 

This means that if I do not make drastic restructurings in the UI, it will be difficult to use optimistic updates in the news list when I e.g. like a news item or similar. Which means that the action after implementing React Query is slower than before. 

This was one of the reasons why I initially wanted to try implementing RQ in Easy Staff as there are a lot of direct actions like this. Maybe there is a solution to this problem but none I have found in this short time.  

 

 
 

5 Experiences and lessons learned 

 

5.1 Lessons learned 

 
After talking to supervisors at Apptech, the biggest experience they have gained from my thesis is to see an interesting new library in action in an application that is out on the market and in combination with their proven architecture and structure. In this way, a possible decision can be made to keep the previous structure or, after more careful evaluation, release the further worked version with React Query to production. 

An experience that I have personally drawn from the project is to evaluate the importance of architecture in larger applications, a bad architecture results in a difficult to maintain code and a less scalable product while a good architecture facilitates further development, reduces maintenance and contributes to a more scalable product. It does not have to be a cool new library that solves such a problem, but the solution may be to look at what you already have with different eyes and restructure.  

If you are going to spend an extreme number of hours inserting a new library into an already functioning application, you should be sure that it helps and does not hinder, because it is not just to do.  

 
5.2 Lessons learned  
 

An important lesson that I have taken with me from this thesis is that a well-designed API is an important factor in facilitating frontend development. If the API is not well designed, the work becomes heavier and can lead to lots of mutated data in the frontend, regardless of how many advanced libraries are used. This, in turn, negatively affects the user experience. I think it is crucial to have good communication between front-end and back-end developers to solve problems, such as where a certain type of logic should be handled or what a data structure should look like for a specific object. 

This lesson has become extra clear to me during the degree project because React Query works best with pure data binding. If this is not the case, then one must begin to mutate the state and perform complex actions to get the library working properly. 

Another lesson I have taken with me is that it is not just to replace logic in an already working application. Before I started with this project, I was probably a little too naïve and had the attitude how hard can it be?  
In retrospect, I have understood the complexity of it.  

The above lessons have broadened my understanding of the concept of programming and emphasized the importance of app development being a team effort based on responsiveness, social skills and a constant drive forward. It is an insight that I think is very nice, it will improve my attitude to future working life as I am definitely a team player who loves social contexts. 

 
 

Purely factually, the biggest lesson I have taken with me is all the knowledge I have gained in React Query, I think this is a library we will see more and more of in the future. The library really simplifies the management of the data warehouse in React applications using its cache. If you then pair it with your own custom hooks, you have a scalable and centralized total solution for the application's server states.  

 

5.3 The project today – development and future 

 
Since Easy Staff is an application that is in production, it is constantly being developed and has also done so while I have been working on my degree project. 

After this report is ready, I will present the results to my colleagues at apptech, together we will consider whether React Query is something to build on in Easy Staff or not.  

Then I will work for us to introduce React Query in our technology stack for future projects. The library is brilliant if you ask me and I think it does even more justice if you implement it from the start in a new project so that the library has a greater chance to interact with the architecture from the start. In this way, you can make full use of the entire library.  

 

 
 

6 References 

 

Den officiella React Query dokumentationen. 

Blog om React Query 

React Query artikell. 

Video om React Query. 

Video with the founder of React Query. 

 

 

 
