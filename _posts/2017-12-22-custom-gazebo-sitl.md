---
title: Custom SITL Solutions for Gazebo
---

Gazebo is a powerful high fidelity simulator. As we discussed in our previous
[post]({% post_url 2017-8-22-gazebo-sitl %}) Betaflight can interact with
Gazebo to create a SITL testing environment. However how things currently exist,
Betaflight is reusing Arducopters Gazebo plugin in and models. In this post well
discuss the methods to take to create your own SITL system.

There are components we are able to customize,
1. World
2. Aircraft model
3. Gazebo SITL plugin

# World

The world is our simulated environment. When Gazebo is launched, we can specify
the world to launch. Again see previous post for how to do this. By default the Arudopilot world loads in
an Iris quadcopter model in an environment with a runway. If we want a more
complex environment to use during simulation, or do load a different aircraft
model this is where we do it. First lets get
the source for the current SITL world, then well modify it from there.

Fork then clone or download the Gazebo repo
[https://bitbucket.org/osrf/gazebo](https://bitbucket.org/osrf/gazebo).

Copy `worlds/iris_arducopter_demo.world` to your workspace in a directory called
`worlds` or `resources`. Open
it with your favorite text editor. This world file is an SDF file which is an XML to describe
robots and their simulations.  In the world we define properties of our world
such as the physics characteristics,   how fast the
simulator should run (i.e., faster than real time) by modifying `max_step_size`
and `real_time_update_rate`, and models to include.  At the bottom
we can see the `model://iris_with_standoffs_demo` is included which contains the
actual Iris quadcopter model. If we want to change the quadcopter model, this is
the URI we need to change. In the next section well see how to go about doing
this. 

# Aircraft Model

After  we define our world we can change the aircraft model we want to use. We
can use the iris demo as a starting point.
Fork then clone or download the Gazebo model repo
[https://bitbucket.org/osrf/gazebo_models](https://bitbucket.org/osrf/gazebo_models). 
Copy `iris_with_standoffs_demo` and `iris_with_standoffs` to your own workspace
under a directory called `models`.
  The next thing we want to do is give our model a unique name otherwise we will
have conflicts with what model components will be used since the Iris demo is
installed by default in Gazebo. This is the name you'll use in your world to
include your custom aircraft. These names are also used in  URIs to access and
include other models. Nested models are delimited by  a double colon "::".
The best way to clean this up is just grep for "iris". 

# Gazebo SITL Plugin



# Tying it all together

Now that we are using our own worlds, models and plugins we need to tell Gazebo
about them. This is done through environment variables.  The easiest way to do
this is create your own bash script. 

According to Gazebos [documentation](http://gazebosim.org/tutorials?tut=components)
you *must* first source the Gazebo setup script otherwise nothing is going to
world. In your script first source the setup script like so,

~~~
source /usr/share/gazebo-8/setup.sh
~~~

Then add your directory paths for your models, worlds, and plugins to the
`GAZEBO_MODEL_PATH`, `GAZEBO_RESOURCE_PATH`, `GAZEBO_PLUGIN_PATH` environment
variables. Then 


This is a sample of my `start_gazebo.sh` file (note I haven't done anything with
the SITL plugin yet), 

~~~
source /usr/share/gazebo-8/setup.sh
export
GAZEBO_MODEL_PATH=/home/wil/workspace/sitl/gazebo/models:${GAZEBO_MODEL_PATH}
export
GAZEBO_RESOURCE_PATH=/home/wil/workspace/sitl/gazebo/worlds:${GAZEBO_RESOURCE_PATH}
gazebo --verbose /home/wil/workspace/sitl/gazebo/worlds/xquad_demo.world
~~~
