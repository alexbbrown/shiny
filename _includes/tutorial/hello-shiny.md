
![Hello Shiny Screenshot](screenshots/hello-shiny.png)

The Hello Shiny example is a simple application that generates a random distribution with a configurable number of observations and then plots it. To run the example type: 

<pre><code class="console">&gt; library(shiny)
&gt; runExample(&quot;01_hello&quot;)
</code></pre>

Shiny applications have two components: a user-interface definition and a server script. The source code for both of these components is listed below. 

In subsequent sections of the tutorial we'll break down all of the code in detail and explain the use of "reactive" functions for generating output. For starters though just try playing with the sample application and reviewing the source code to get an initial feel for things. Be sure to read the comments carefully as they explain in detail what the code is doing and why.

The user interface is defined in a source file named ui.R:

#### ui.R

<pre><code class="r">library(shiny)

# Define UI for application that plots random distributions 
shinyUI(pageWithSidebar(

  # Application title
  headerPanel(&quot;Hello Shiny!&quot;),

  # Sidebar with a slider input for number of observations
  sidebarPanel(
    sliderInput(&quot;obs&quot;, 
                &quot;Number of observations:&quot;, 
                min = 0, 
                max = 1000, 
                value = 500)
  ),

  # Show a plot of the generated distribution
  mainPanel(
    plotOutput(&quot;distPlot&quot;)
  )
))
</code></pre>

The server-side of the application is shown below. At one level it's very simple--a random distribution with the requested number of observations is generated and then plotted as a historgram. However, you'll also notice that the function which returns the plot is wrapped in a call to `reactivePlot`. The comment above the function explains a bit about this, but if you find it confusing don't worry we'll cover this concept in much more detail soon.

#### server.R

<pre><code class="r">library(shiny)

# Define server logic required to generate and plot a random distribution
shinyServer(function(input, output) {

  # Function that generates a plot of the distribution. The function
  # is wrapped in a call to reactivePlot to indicate that:
  #
  #  1) It is &quot;reactive&quot; and therefore should be automatically 
  #     re-executed when inputs change
  #  2) Its output type is a plot 
  #
  output$distPlot &lt;- reactivePlot(function() {

    # generate an rnorm distribution and plot it
    dist &lt;- rnorm(input$obs)
    hist(dist)
  })
})
</code></pre>

The next example will show the use more input controls as well as the use of reactive functions to generate textual output.