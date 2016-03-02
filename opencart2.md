# opencart 笔记2 (Theme and Module Development) #

## Themes ##



### About opencart Themes ###

  * OpenCart uses MVC architecture. This means that the "V" or "view" is kept separate from the main code.
  * The "view" is made up of the **.tpl files and are always found in: catalog/view/theme/YOUR\_THEME/**.*** tpl files are mostly basic html files with some php calls to display the data that you want to see.
  * There is no template engine like "Smarty". Template engines are popular buzz words but only complicate and limit your usage. PHP by default is an embeddable language and lets you do everything you need with much less code than a template engine.
  * references to php tags in the tpl files are based on the controllers available variables, or global class references. The syntax is simple
  * In the controller put something like $this->data['variable'] = 'xyz';**

  * In the tpl file you would reference that with:  <?php echo $variable; ?>



OpenCart supports multiple catalog themes. There are many themes available from the contributions are as well as commercially. You can also choose to create your own theme.

### How to Create a new Theme ###

There are some golden rules to follow when creating your own theme.

  * NEVER  edit the files in the "default" theme directly. This is your base and fallback. When new upgrades are released, this folder gets overwritten with the latest code. <br>
</li></ul><ul><li>NEVER copy the entire "default" folder and just rename it. This will make upgrading much more difficult. OpenCart uses a "default theme fallback system". This means that if you are missing a file in your custom theme folder, it will search the main "default" theme folder for the file. The great benefit of this is that you only need to edit very few files to make a completely different looking theme, and when there are new versions released, your theme will fallback to the newly changed default versions which makes your theme virtually transparent during an upgrade.<br></li></ul>



  * Create a new theme folder in the catalog/view/theme path. For this example, I will use the name "silverfish"
  * Copy the following files from the "default" theme, into the "silverfish" folder. Be sure to keep the same directory structure:
> - catalog/view/theme/default /stylesheet/**.** <br>
<blockquote>- catalog/view/theme/default /image/<b>.</b> <br>
- catalog/view/theme/default /template/common/header.tpl  <br>
</blockquote><ul><li>Edit the catalog/view/theme/silverfish /template/common/header.tpl<br>
</li><li>Find and replace all references to "default" with "silverfish" in that file. Save & Close.<br>
</li><li>From the OpenCart Admin area, Goto the main Settings section and change the theme from default to silverfish<br>
<ul><li>Edit the catalog/view/theme/silverfish/stylesheet/stylesheet.css file<br>
</li></ul></li><li>Make a change to the background color or whatever you like<br>
</li><li>Now reload the homepage. You should see the new changes.<br>
</li><li>Only copy files from the "default" folder as needed when making custom changes to the structure of those pages.</li></ul>



When upgrading, your theme should be untouched. Any new changes in the default template will be shown while using your template with the exception of any changes to the files that you've customized. For example, if the catalog/view/theme/default/template/common/header.tpl file was changed, then you will need to make the small adjustments to the one in your theme. But that is much easier than trying to recreate your theme.<br>
<br>
<br>
<br>
<br>
<br>
<h1>Modules</h1>
<h2>Overview</h2>

OpenCart utilizes a Model-View-Controller (MVC) structure. MVC separates code by concern, allowing developers to maintain and extend small segments of code specific to a given function verses a plethora of nested, interdependent files. It is highly recommended that you familiarize yourself with the form and function of MVC before proceeding to modify and extend OpenCart. For more information regarding MVC, please refer to Wikipedia .<br>
Where to Start<br>
<br>
We start with the files that are needed in order to create your module.<br>
<br>
<br>
<br>
For the admin section:<br>
<br>
<blockquote>admin/controller/module/module_name.php<br>
admin/language/english/module/module_name.php<br>
admin/view/template/module/module_name.tpl</blockquote>

For the front end(if required by your module):<br>
<br>
<blockquote>catalog/controller/module/module_name.php<br>
catalog/language/english/module/module_name.php<br>
catalog/view/theme/default/template/module/module_name.tpl</blockquote>

The above are the main files that would be used by your module, so in total 6 files. Next, we should touch on each one and explain in simple terms what they are used for.<br>
<br>
<h2>The Admin Controller</h2>

This is the controller file for the admin section. For those of you who are not familiar with the use of this file, I will try to explain it clearly here. This file is the building block or your module (admin and catalog). This file does not produce anything to the end user, it 'constructs' the data that will be passed to the view file. Essentially, the controller is a PHP class and nothing more.<br>
<br>
So what must you do to create this controller file? First you need to start the class:<br>
<br>
<pre><code><br>
&lt;?php class ControllerModuleName extends Controller {<br>
<br>
    private $error = array();<br>
    public function index() {<br>
<br>
         <br>
<br>
    }<br>
<br>
} ?&gt;<br>
<br>
</code></pre>

That is the basis of your Controller. Now you have the use of the built in error control and the start of your modules controller. Of course this will not work without it having us enter the rest of the code. Not to mention, what language file should be used, how is this module going to be displayed...still a few more steps!<br>
<br>
So let's get back to the basics of your admin controller.<br>
<br>
<br>
<pre><code>&lt;?php class ControllerModuleName extends Controller {<br>
<br>
    private $error = array();<br>
    public function index() {<br>
<br>
        $this-&gt;load-&gt;language('module/modulename'); // THIS IS LOCATED UNDER YOUR ADMIN DIRECTORY <br>
        $this-&gt;document-&gt;title = $this-&gt;language-&gt;get('heading_title');<br>
<br>
    }<br>
<br>
} ?&gt;<br>
<br>
</code></pre>

Now what we have done here is said that there is a language file located in admin/language/module/modulename.php. Notice that with the PHP Framework you do not need to include the .php. Next we are grabbing the 'heading_title' from the language file so that it stores the title in document->title(which will be used in the view of your module).<br>
<br>
Moving on...if you want to utilize any of the built in OpenCart features make sure to include the following in your code (which is admin/controller/setting/setting.php):<br>
<br>
<pre><code><br>
&lt;?php class ControllerModuleName extends Controller {<br>
<br>
    private $error = array();<br>
    public function index() {<br>
<br>
        $this-&gt;load-&gt;language('module/modulename'); // THIS IS LOCATED UNDER YOUR ADMIN DIRECTORY<br>
        $this-&gt;document-&gt;title = $this-&gt;language-&gt;get('heading_title');<br>
        $this-&gt;load-&gt;model('setting/setting'); <br>
<br>
    }<br>
<br>
} ?&gt;<br>
</code></pre>