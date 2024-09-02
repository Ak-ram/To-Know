Video 1:
Chrome introduced a powerful dev tool for debugging with muliple tabs : 
1- sensor panel to simulate the orientation of a mobile device.
2- performance panel to find bottlenecks in your website.
And so on, and in this image all panels available to use in chrome:

￼
Yellow colors: main panels 
Others : secondary panels

video 2:
What is chrome devtools ? 
It’s a powerful tools included in chrome help developer to debug websites through bunch of panels: 1- Device mode: to display website in different screens.
2- Elements: to debug html/css for each elements in the dom.
3- Sources: to debug js code and add breakpoints and follow function stack.
4- network: to monitor requests and responses and see request time and which one is fail or success.
5- Performance: to test the performance of the page and see how much time it take and how to improve it.
6- Memory: to check memory usage of user website ‘how much memory your website need to run’
7- Application: to display loaded ‘cashed’ resources of your webpage like images, fonts, sessions, cookies, storage, indexedDB
8- Security: to display certificates of your website like SSL certificate and TLC certificate 
We will go through each on in the next videos.

Video 3:

How to open DEVtools?

1- to open devtool on elements panel : cmd/ctrl + option/shift + c 
2- to open devtool on console panel : cmd/ctrl + option/shift + j
3- to open the last panel you visited: cmd/ctrl + option/shift + I
4- if you want to automatically open dev tool with each chrome tab you open then do this:    4.1- in chrome icon right click and choose properties and change target to “--auto-open-devtools-for-tabs”  



Video 4:

In vscode we have a command palette to run specific commands “cmd+option+p”  also in devtools we have a command menu to execute some tasks  “cmd+shift+p” or “click on 3 dots in devtool and choose Run command”, 
This menu can :
1- Run>command (ex, hard reload, screenshot, open file)
2- Open File (filename)
3- Help? 


Video 5:

In this video we take a close look at device mode tab to test website in different mobiles screens based on mobile type and you can also add a specific device with custom dimentsion also you can press on bottom arrow in the top right part to perform specific actions like 

1. 1- Test Responsiveness of you web page in different mobile sizes.
2. 2- You can add specific device screen with custom dimensions.
3. 3- You can view webpage in both portal and landscape modes
4. 4- You can perform another options by clicking on 3 dots in the top right section like:  4.1 - Show media queries break points
5. 4.2 - Take screenshot for this device screen
6. 4.3- Take screenshot for the whole page
7. 4.4- Change device type "mobile, desktop" and determine which action should goes "touch or click"
8. 4.5 display device frame or not
9. 4.6 show rulers in px unit or not


Video 6:

You can also simulate conditions of low connectivity “low network” and poor performance “low cpu” this Is can be done by using throttling options in device mode panel, there are 3 options:

1- No throttling: means keep the same cpu and network speed of the current device “laptop”
2- Middle Tire Mobile: means use “a fast 3G network” and “1/4 capacity of your physical cpu”
3- Low end mobile: means use “a low 3G network” and “1/6 of your cpu capacity”
So these options based on your cpu capacity and network latency 
4- Offline
YOU Can test this on speediest.net to show download/upload speed on each throttling option and also in https://cpu.userbenchmark.com/Software to test cpu capacity on different throttling options.

But these options low down both cpu and network together… what if we need to slow down only network or slow down only cpu? 1- go to performance tab
2- click setting icon and selecting one of the options from the drop down menus ‘cpu select box’
OR you can create a new network limitation profile to set an upload, and download speed and even latency if you need. 
By these steps you can create a new throttling rather than the default existing ones  
￼
Video 7:

Elements TAB:

You can inspect html/css elements from this tab, also you can run “show rulers on hover” command from command menu to  display rulers on hover each element and this is useful to see it’s dimensions also you can scroll into view for a specific element by right clicking on it in the element tab and choose scroll into view option.

Video 8:

You can rearrange elements in the Dom by drag and drop it, also you can set state for the node like if you have anchor element then you can right click on it and choose force state option, this state include these options:   :active, :visited, :hovered, …and so on
In case you select :visited then the text color of anchor element will be violet

Also you can hide / display specific node by clicking on this node in Element Tab and choose hide element or press ‘h’ key. Where as you need to delete it select the node and click on ‘delete’ key or just choose delete node option and press ‘cmd + z’ to undo deleting it  

Video 9:
You can access node in console panel by using ‘$0’, also you can copy js path of the node by choosing copy js path

Video 10:

You can also display the properties that each element takes like “href, label, src, htmlFor” and so on by selecting an element and choosing  properties tab “beside style tab”
You can notice that there Is a bold properties which refers to the properties that the developer bind to the element itself, where are the normal fond properties refer to the properties that the element inherits from the prototype string, also you can search for a specific one by using filter box,

Note these properties not has properties with false value like null, undefined so to show the complete list of properties you should check show all box. 

Video 11:
When selecting node form Elements Panel, you can see that there is a new sub panel opens with options like “style, computed, layout, event listeners, properties, Dom breakpoints, accessibility” tabs,

In style tab you can add new styles, toggle classes and change element status like “:hover” and also you can show/hide computed styles sidebar to display “element box model”


Video 12/13:
Layout tab in Elements Panel is useful in case of Grid/Flexbox elements as it enables us to inspect these elements by draw layout around it, and control which options showed be displayed or not in the layout like “change color of the outlines, display edges number, display areas name and so on…”

Video 14/15: Console Panel

You can log multiple console msg In this panel like “info, error, warns, violation”, info msg include tables, groups, custom console msg, also you can run js code inside it.

Often It called REPL console: READ, Evaluate, Print, Loop
The REPL (Read-Eval-Print Loop) console in DevTools is a powerful interactive environment that allows developers to write and execute JavaScript code in real time within the context of the current web page.

Video 16:

The "Create Live Expression" ‘eye icon’ feature in the Console tab of DevTools allows you to create a live-updating display of a JavaScript expression. Once you create a live expression, it continuously evaluates the expression in real-time and updates its value in the console as the underlying data changes.
If you create Date.now() it will updating the value with real-time
 
Video 17:

Here’s a concise guide to formatting and styling console messages using `console.log`:

### Format Specifiers:
- **Integers**: `%d`
- **Floating-Point Numbers**: `%f`
- **Strings**: `%s`
- **CSS Styling**: `%c`

### Examples:

1. **Format Integers and Strings**:
   ```javascript
   console.log("I have %d apples and %d oranges. I like %s.", 42, 7, "Apples");
   ```

2. **Format Floating-Point Numbers**:
   ```javascript
   console.log("Pi is approximately %f.", 3.14159);
   ```

3. **Apply CSS Styling**:
   ```javascript
   console.log("%cThis is styled text!", "color: red; font-weight: bold;");
   ```

4. **Combine Specifiers**:
   ```javascript
   console.log("User: %s, Age: %d%cCongrats!", "Alice", 30, "%c", "color: green;");
   ```

This approach lets you dynamically insert values and style messages in the console.

Video 18: Network panel
Is useful to verify if resources uploaded and downloaded related to this website successfully also to verify properties of these resources like name, type, imitator “source code that ask for this request”, size, HTTP header and waterfall “it’s a graphical representation of the different stages of the request, you can hover it to see more details”

You can hide or display more columns that display more info about requests by right click and choose header options  at the top we can see timeline section called “overview” that shows in total how long the site took to load as well the resources of the site according to their load times, it’s important to track which resources take a long time to load by slide mouse wheel or select a region on this timeline.

You can view or hide this timeline by go to settings in this tab and checkbox view overview, if you hide overview timeline the resources will be duplicated each time you refresh the page so to prevent these duplication you uncheck the box called ‘preserve log’ on the top of this panel.   You can search for resource by using filter box and If you want to not show results in this filter box you can check
Invert checkbox “it means display all resources except that one in the filter box”, also we can filter resources according to their types by using filtering buttons like JS, CSS, Img, Media and so on…. 

Network panel only track resources while the panel should open otherwise refresh the page  If you want to search for files according to the information in the HTTP request headers and responses,
you can click on this search button, which allows us to search for a string or a regular expression.
For example, let's search for those resources that have a get request. “method: GET”


You can also take a screenshot to the page while it’s loading by clicking on settings option and choose ‘capture screenshots’ of your page while fetching these resources  FINALLY: 
it is possible to block resources to see how the website would behave without them.
To do this, just go to the command menu and select the option “Show request blocking”.
Which will open a new tab where we can click on + button to add a pattern to block the requests that match with it.
For example, let's see what happens if we block the css resources of the site. “*.css”

 Video 19: Sensors panel
With sensors feature you can simulate user location ‘change your location’ and based on it you can display specific data in the UI for example if you develop a weather app that should automatically display weather data based on user location so if he in Egypt then weather data will be related to Egypt and so on. to make sure that this feature works well you can debug it with sensor tab where you can change the location to make sure that the weather data also changed….   to open sensors => in command menu type > show sensors  also with sensors you can simulate device orientation and by the angle of orientation you can display or hide specific elements, so if you hide elements in landscape you can test that with this feature to make sure it’s hidden in a right angle    Video 19: Remote Debugging —skip

 Video 20: Setting options and customizing Devtools
Customize dev tool panel like theme, language,…

 Video 21: Accessiblity 

The main goal of making a site accessible is to ensure that all users, including those with disabilities, can access, use, and benefit from the website’s content and functionality. 

To make sure that your website is accessible by all users you can test it in lighthouse audit tab and check the accessibility checkbox and reload the page, then chrome will give you a report about the how much your website accessible and how to improve it.


Accessibility problmes:  1- low contracts text
You can detect such problem with 3 ways:
- Css overview: open command menu and look for css overview it will open a panel with some tabs click on colors tab to see contracts
- Go to settings then in experiments tab check this option “Enable automatic contrast issue reporting via the issues panel” then reload the page and open command menu and look for ‘issues’ panel
- Lighthouse: you can check accessibility option and start analyzing and check if there is any contrast issues

After detecting the elements that have contrast problem, we can solve it by:    1- inspect the element and hover it on the ui will display a list of properties of this element and you can see a warning icon for contrast so you should change the color to another one

2- unaccessible element when navigating with `tab` key
 to solve this problem open the console and add expression to watch the current selecting element by clicking on eye icon and add this line:  “ document.activeElement “ this will display the current selected element and if there is any unfocused element when pressing tab then you should work on it.



 Video 22: Recorder Panel
 on command menu search for recorder, this panel allows you to record user interacts on your website so it’s helpful to improve ux, troubleshoot problems or measure performance between flow steps.

Recorder will automatically detect elements with these selectors: 
1- css 2- ARIA 3- TEXT 4- Path 5-Pierce
You can track any type by unchecking these checkboxes or you can type the selector (element) that you want to record in the selector attribute input, you can start recording and interact with the page and then end the recording and you will get a recorded video that illustrates each interactions that the user take in the page, you can test performance for each step, you can delete step or add one, you can delete the whole recorder or export it indifferent format, you can import a previously recorded file, you can add breakpoints for a specific step “interact” and you can test performance for each step to now the bottleneck 

 Video 23: Lighthouse Panel  this panel enables you to generate report about how your website works in case of “performance”, “SEO”, “Accessibility”, “Best Practices” as well as give suggestions on points that need to be improved listed in Opportunities and diagnostic sections

Video 23: Animatins Panel
There is a tool that inspect and modify animations called “animation” and you can find it in the command menu it will open animation tab that will record animations of the page when it occurs, you can find controls to modify the speed of animation “100%”, “50%”, “25%”, and bellow you can see captures for animation at a specific points, you can click on each capture to remove it or modify it by manipulating the timeline bar for each animated item 

Video 23: Track changes Panel
Open command menu and search for changes which will open a changes tab, then navigate to sources panel and choose overrides tab in the sidebar and add a folder “this folder where all your changes will save in” and then everything you changes will be tracked and appear in changes panel and when saving the file these changes will saved in the folder you set before.

In case of css you can change css content form elements panel and these changes will tracked to changes panel also 

Also you can find a copy button in the bottom of changes tab If you need to copy changes you made for a specific file.

Video 23: unused css and js
You can track unused css and js by using coverage panel, you can find it in command menu tab after openning it you should click on reload button which will display a list of resources used in this website, and display also data related to each resources like url, type, total bytes, unusedbytes, usage visual bar that consist of 2 colors red and green to indicate how much used and unused js/css code , if you click on a specific resource will navigate  you to editor that displays the code in this file and also a red line in the left to indicate unused code

Video 23: css overview
This dispaly information about the used css properties like used colors, fonts, media quires and also unused css declaration.


Video 23: find & fix problems

There are some issues that don’t appear as a messages in console tab but it show in issues tab where you can find info about the issue like how to solve it and the resource that make this issue.


Video 23: Media tab

Open command menu and write media to open a tab that analyzes media files and give you detailed info about messages, properties, and events in this media that can be exported in json file for further analysis.


Video 23: Security Panel

This panel help you to check the certificates of the website and see it, also tell you if the website is secure ‘https’ or not ‘http’, you should refresh the page to make this tab display info exactly like in network tab

Video 23: Search Panel
You can search for a string or element in all loaded files within this tab, just open command menu and look for search command


Video 23: Rendering Panel
Open command menu and look for rendering, this will open a tab with a list of checkboxes for specific options:
1- Pain Flashing: this detect elements that are repeatedly re-rendered or updated
2- Layout Shift Regions:  highlight element when change its position from point to point.
3- Frame Rendering stats: give a real time estimiation of frames per second.
We can also see information on GPU frame status and GPU memory usage.


Video 24: Application tab:
In this panel you can find several tabs like:  1- manifest: for PWA
2- storage: display used storage by this site and also clear site data option
3- local storage : to store data in user’s device all the time
4- session storage: to store data in user’s device, and this data released automatically when he close the tab.
5- cookies:  is a small piece of data that is sent from a website and stored on the user's device.
Cookies are used to remember information about the user, such as their preferences or what is in their
shopping cart.
6- cache storage: list all resources that the website cashed
7- indexedDB: 

Video 24: Memory tab:
Detached elements: In JavaScript, a detached element refers to an element that exists in the memory of your program but is no longer attached to the DOM (Document Object Model) tree. This means the element was once part of the document, but has been removed or "detached" from the visible page structure.

// Creating an element
const div = document.createElement('div');

// The div is currently detached because it's not in the DOM
console.log(div.parentNode); // null

// Adding the element to the DOM
document.body.appendChild(div);

// Now it's attached
console.log(div.parentNode); // <body>

// Removing the element from the DOM
document.body.removeChild(div);

// The div is now detached again
console.log(div.parentNode); // null


We. Can detect detached elements from memory tab, click on Heap snapshot and it will display detached elements in the bottom section then you can click on take a snapshot button to take a snapshot to the whole objects inside the heap, but we need only detached elements so you can write ‘detached’ in filter box to display only detached elements and now where this detached element exist in your code and solve the problem


You can also track when memory allocating occurs by using ‘allocation instrumentation on timeline” let say we have a button that store value in a variable and you need to track this memory allocation so you should navigate to memory tab and check this box “allocation instrumentation on timeline” and click on start button in the bottom it will start recording to detect any allocation occurs while recording is running so If you click on the button you created it will display a blue line in the timeline that tell use there are a memory allocation occurs also you can select this blue line in the timeline to display which variable allocated in the memory

