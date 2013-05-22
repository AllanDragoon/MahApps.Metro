---
layout: default
title: Getting Started - MahApps.Metro
---

##Getting Started

- [Installing MahApps.Metro](#installing_mahappsmetro)
- [Styling a Window](#styling_a_window)
- [Explaining the MetroWindow elements](#explaining_the_metrowindow_elements)
- [Customisation](#customisation)
- [Advanced](#advanced)

## Installation

You can install MahApps.Metro via Nuget using the GUI (right click on your project, Manage Nuget References, search for 'MahApps.Metro') or via the console:

<pre class="nuget-button">Install-Package MahApps.Metro</pre>

If you wish to use the *alpha* releases of MahApps.Metro, you need to include "pre" releases in Nuget (1.7 and above) 

![](images/include_prerelease.png)

or use the console (`PM> Install-Package MahApps.Metro -Pre`)

## Styling a Window

There are two main approaches you can take with MahApps.Metro to style a Window, using the `MetroWindow` control, or rolling your own. For the getting started guide, we'll use `MetroWindow`, as this approach will work for a good percentage of apps and is the quickest and easiest way to get going. Rolling your own styling is covered in the later "Advanced" section.

A default WPF Window with a few controls looks like the following:

![](/images/01_UnstyledWindow.png)

After installing MahApps.Metro:

 - open up `MainWindow.xaml`
 - add a namespace reference in the opening Window tag:  
`xmlns:Controls="clr-namespace:MahApps.Metro.Controls;assembly=MahApps.Metro"`
 - change `<Window ...` to `<Controls:MetroWindow ...` (remember to change the closing tag!)

You'll need to change the codebehind `MainWindow.xaml.cs` file (right-click the designer and select 'View Code' or press `F7`) so that the base class for `MainWindow` matches the new XAML tree:

    public partial class MainWindow : MetroWindow
    {

    }

In most cases you can just drop the inheritance on a partial declaration:

    public partial class MainWindow
    {
    	
    }

 This basic MetroWindow will look like this:

![](/images/02_PartiallyStyledWindow.png)

This doesn't look very 'metro'-ish yet because the resources and styles need to be included. Unfortunately you need to include these resources in each Window.

> We have tried embedding the resources and styles in `MetroWindow`, but then you lose all ability to dynamically change the theme.

Just under the opening MetroWindow tag, add the following
	
	   <Window.Resources>
	        <ResourceDictionary>
	            <ResourceDictionary.MergedDictionaries>
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Colours.xaml" />
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Fonts.xaml" />
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Controls.xaml" />
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/Blue.xaml" />
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/BaseLight.xaml" />
	            </ResourceDictionary.MergedDictionaries>
	        </ResourceDictionary>
	    </Window.Resources>
	
![](/images/03_StyledWindow.png)

And now we have a semi-decent looking window!

## Explaining the MetroWindow elements

![](/images/04_ExplainedStyledWindow.png)

If you don't like the elements that are labelled, fear not, they're all optional.

1. The titlebar is what sets `MetroWindow` apart from rolling your own. `ShowTitleBar="true|false"`
2. Instead of using static images, this uses a font called **Marlett**.
3. The resize grip is no longer (from 0.8 onwards) the *only* way to resize a `MetroWindow` - all edges and corners can be gripped, but given a metro window doesn't have a noticeable window "chrome" like an aero window, the resize grip can help reassure users
4. `ShowIconOnTitleBar="true|false"` 

## Customisation

### WindowCommands

`WindowCommands` are the minimise, maximise/restore, and close buttons. You can add your own controls to `WindowsCommands` - by default, buttons have a style automatically applied to them to make them fit in with the rest of the `WindowsCommands`. As of 0.9, you are no longer limited to just buttons, but any control. Be aware, you're responsible for styling anything other than buttons.

Including this within the `MetroWindow` tag (under the `Window.Resources` section),

	<Controls:MetroWindow.WindowCommands>
	    <Controls:WindowCommands>
	        <Button Content="settings" />
            <Button>
                <StackPanel Orientation="Horizontal">
                    <Rectangle Width="20" Height="20">
                        <Rectangle.Resources>
                            <SolidColorBrush x:Key="BlackBrush" Color="White" />
                        </Rectangle.Resources>
                        <Rectangle.Fill>
                            <VisualBrush Stretch="Fill" Visual="{StaticResource appbar_cupcake}" />
                        </Rectangle.Fill>
                    </Rectangle>
                    <TextBlock Text="deploy cupcakes" />
                </StackPanel>
            </Button>
        </Controls:WindowCommands>
	</Controls:MetroWindow.WindowCommands>

> Make sure to include the [icons](#icons) to get the cupcake

Produces this window titlebar:

![](/images/05_WindowCommands.png)

The foreground (link) colour of `WindowCommands` will always be white, *unless* the titlebar is disabled, in which case it will be the reverse of whatever theme you have selected. For example, using the White/Light theme, the foreground colour will be black.

## Advanced

### Roll your own Window

The roll your own approach is very relevant, depending on what style of app you're going for. A good example is [code52's MarkPad](http://code52.org/DownmarkerWPF/), where we needed the flexibility of rolling our own window while still using MahApps.Metro as an underlying visual framework. The key elements used were the `WindowCommands` (discussed [above](#windowcommands)), and `BorderlessWindowBehavior`.

#### BorderlessWindowBehavior

Add some namespace references to the opening `Window` tag:

	xmlns:i="http://schemas.microsoft.com/expression/2010/interactivity"
	xmlns:Behaviours="clr-namespace:MahApps.Metro.Behaviours;assembly=MahApps.Metro"

Then somewhere within the `Window`, add

    <i:Interaction.Behaviors>
        <Behaviours:BorderlessWindowBehavior/>
    </i:Interaction.Behaviors>

And you're done! `BorderlessWindowBehavior` has a few options, such as `ResizeWithGrip` and `AutoSizeToContent`:

	<Behaviours:BorderlessWindowBehavior ResizeWithGrip="False" />