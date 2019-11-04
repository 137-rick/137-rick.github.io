---
id: 129
title: dev for openfire with eclipse
date: 2011-05-23T11:08:25+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/129
permalink: /archives/129
categories:
  - Android
  - 经典技巧
tags:
  - eclipse
  - it
  - openfire
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  <h3>
    <span STYLE="color:#666699;">Install JDK</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        Download JDK and install them. The least version should be 1.5.I use 1.6. Sorry, no instruction for this.
      </p>
    </li>
  </ul>
  
  <h3>
    <span STYLE="color:#666699;">Install Eclipse 3.3</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        Download Eclipse 3.3 from www.eclipse.org. I use <em>Eclipse IDEfor Java EE Developers</em>. You should at least use <em>EclipseIDE for Java Developers</em>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Extract the downloaded zip file into <strong>C:/ProgramFiles/Eclipse</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Open <strong>C:/Program Files/Eclipse</strong> folder.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Right click and drag <strong>eclipse.exe</strong> on to yourdesktop (or Windows taskbar) to create a shortcut icon.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Right click the shortcut icon and choose<strong>Properties</strong>. The <strong>EclipseProperties</strong> window will show.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        The <strong>Target</strong> textbox should read something likethis <strong>&#8220;C:\Program Files\Eclipse\eclipse.exe&#8221; -vm &#8220;C:\ProgramFiles\Java\jdk1.6.0\bin\javaw&#8221;</strong> depending on the JDK thatyou use and where you installed it.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Close the <strong>Eclipse Properties</strong> window.
      </p>
    </li>
  </ul>
  
  <h3>
    <span STYLE="color:#666699;">Install SubversivePlugin</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        Double-click the shortcut icon to start Eclipse.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Select/enter your preferred workspace and click<strong>OK</strong> to open Eclipse main IDE window.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on the <strong>Workbench</strong> icon to close thewelcome screen.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click <strong>Help::Software Updates::Find andInstall&#8230;</strong> menu.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on <strong>Search for new features to install</strong> andclick <strong>Next</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on <strong>New Remote Site&#8230;</strong> button.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Enter <strong>Subversive</strong> in the <strong>Name</strong>box and <strong><a HREF="http://www.polarion.org/projects/subversive/download/1.1/update-site" TARGET="_blank">http://www.polarion.org/projects/subversive/download/1.1/update-site</a></strong>in the URL box (Check the latest URL from <a HREF="http://www.eclipse.org/subversive">http://www.eclipse.org/subversive</a>website), then click <strong>OK</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click <strong>Finish</strong> to install Subversive. Eclipsewill search for the update site and show the result in a nextwindow where you will select the features to install. I chooseeverything under <strong>Subversive SVN Team ProviderPlugin</strong> and <strong>Subversive ClientLibraries</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click <strong>Next</strong> to continue and so on until theinstallation ends. You normally want to restart Eclipse when theinstallation ends.
      </p>
    </li>
  </ul>
  
  <h3>
    <span STYLE="color:#666699;">Check Out Openfire SVN</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        Click <strong>Windows::Open Perspective::Other&#8230;</strong>menu.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on <strong>SVN Repository Exploring</strong> on the<strong>Open Perspective</strong> window and click<strong>OK</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Right-click on <strong>SVN Repositories</strong> screen andchoose <strong>New::Repository Location&#8230;</strong>
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        On <strong>New Repository Location</strong> enter<strong><a HREF="http://svn.igniterealtime.org/svn/repos" TARGET="_blank">http://svn.igniterealtime.org/svn/repos</a></strong> inthe URL box and click <strong>Finish</strong>. You&#8217;ll see the URLlocation in the <strong>SVN Repositories</strong> screen.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Expand the URL location.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Expand the <strong>openfire</strong> tree.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Right-click on <strong>trunk</strong> and choose <strong>CheckOut</strong>. Make yourself some Cafe-Latte while waiting for thecheckout to complete. <img src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://community.igniterealtime.org/4.5.5/images/emoticons/happy.gif" WIDTH="16px" HEIGHT="16px" TITLE="dev for openfire with eclipse" />
      </p>
    </li>
  </ul>
  
  <h3>
    <span STYLE="color:#666699;">Create OpenfireProject</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        Click <strong>Window::Open Perspective::Java</strong> menu.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        In the <strong>Project Explorer</strong> screen, if there is an<strong>openfire</strong> project, delete it. This project wascreated during the Openfire check out process. Yes you read itcorrectly, DELETE the project!!! Otherwise you&#8217;ll have to setupyour Openfire development environment manually. On the<strong>Confirm Project Delete</strong> choose <strong>Do notdelete contents</strong>, then click <strong>Yes</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click <strong>File::New::Project&#8230;</strong> Notice theellipses!!!
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Select <strong>Java::Java Project</strong> and click<strong>Next</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        On the <strong>New Java Project</strong> window choose<strong>Create project from existing source</strong> and browse towhere <strong>openfire</strong> folder is located under yourworkspace.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        In the <strong>Project</strong> <strong>name</strong> box enterexactly as <strong>openfire</strong>. Otherwise, the<strong>Next</strong> and <strong>Finish</strong> button remaindisabled. Click on <strong>Next</strong>. Eclipse will read thedirectory structure to setup the environment automatically (almost)for you and you can see what it does on the next screen. Then clickon <strong>Finish</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        If the <strong>Open Associated Perspective</strong> windowsopens, click Yes.
      </p>
    </li>
  </ul>
  
  <h3>
    <span STYLE="color:#666699;">Build Openfire</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        Click <strong>Window::Show View::Ant</strong> menu.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Right-click the <strong>Ant</strong> screen and choose<strong>Add Buildfiles&#8230;</strong>
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Expand the <strong>openfire::build</strong> folder and select<strong>build.xml</strong>, then click <strong>OK</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        On the <strong>Ant</strong> screen, expand the <strong>OpenfireXMPP Server</strong> and double-click on <strong>openfire</strong>ant task. The build may fail because you&#8217;re checking out the dailyupdates of Openfire sources, which may contain bugs. If so, waitfor another day and hope that the developers discover and fix thebug; or you might dare to fix it yourself. During this first timesetup, a successful build is necessary before you can proceed withthe remaining tasks below.
      </p>
    </li>
  </ul>
  
  <h3>
    <span STYLE="color:#666699;">Create Project Builder</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        Click <strong>Run::Open Run Dialog&#8230;</strong> or<strong>Run::Open Debug Dialog&#8230;</strong> menu. A<strong>Run</strong> window shows.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Select <strong>Java Application</strong> and click on the<strong>New</strong> button.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        On the <strong>Main</strong> tab of the <strong>Run</strong>window, change the <strong>New_configuration</strong> name to<strong>Openfire</strong> or anything you like.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on <strong>Project::Browse</strong> button and select<strong>openfire</strong> and click <strong>OK</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on <strong>Main class::Search</strong> button and select<strong>ServerStarter &#8211; org.jivesoftware.openfire.starter</strong>and click <strong>OK</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        I&#8217;d suggest that you select <strong>Stop in main</strong> checkbox so that you could later verify that debugging works.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on the <strong>Arguments</strong> tab.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Enter<strong>-DopenfireHome=&#8221;${workspace_loc:openfire}/target/openfire&#8221;</strong>in the <strong>VM arguments</strong> box.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on <strong>Classpath</strong> tab.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Select <strong>User Entries</strong> so that the<strong>Advanced&#8230;</strong> button will be enabled.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on the <strong>Advanced&#8230;</strong> button.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        On the <strong>Advanced Options</strong> window select<strong>Add Folders</strong> and click <strong>OK</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        On the <strong>Folder Selection</strong> window select<strong>openfire::src::i18n</strong> folder and click<strong>OK</strong>.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on the <strong>Advanced&#8230;</strong> and <strong>AddFolders</strong> buttons once again to include<strong>openfire::src::resources::jar</strong> folder.
      </p>
    </li>
    
    <li TYPE="ul">
      Click on the <strong>Advanced&#8230;</strong> and<strong>Add Folders</strong> buttons once again to include<strong>openfire::build::lib::dist</strong> folder.
    </li>
    <li TYPE="ul">
      <p>
        Click on <strong>Common</strong> tab.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Select the <strong>Debug</strong> and <strong>Run</strong> checkbox.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on <strong>Apply</strong> button.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        Click on <strong>Close</strong> button.
      </p>
    </li>
  </ul>
  
  <h3>
    <span STYLE="color:#666699;">Run/Debug</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        The setting is now complete for Openfire.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        You may test running and debugging by clicking on<strong>Run::Run History::Openfire</strong> and <strong>Run::DebugHistory::Openfire</strong> respectively. If you choose the laterand if you follow this instruction closely, execution will stop onthe main method in <strong>ServerStarter.java</strong>.
      </p>
    </li>
  </ul>
  
  <h3>
    <span STYLE="color:#666699;">Plugin Setup</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        Please refer to <a HREF="http://community.igniterealtime.org/docs/DOC-1200">OpenfirePlugins Setup Guide For Eclipse</a>
      </p>
    </li>
  </ul>
  
  <h3>
    <span STYLE="color:#666699;">Related Documents</span>
  </h3>
  
  <ul>
    <li TYPE="ul">
      <p>
        <a HREF="http://community.igniterealtime.org/docs/DOC-1200">OpenfirePlugins Setup Guide For Eclipse</a>
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        <a HREF="http://community.igniterealtime.org/docs/DOC-1022">Openfire.tar.gz + Eclipse 3.1 Installation Guide</a> &#8211; Installation guidefor those who prefer to download the source code in tarballformat.
      </p>
    </li>
    
    <li TYPE="ul">
      <p>
        <a HREF="http://community.igniterealtime.org/docs/DOC-1040">Spark SVN +Eclipse 3.3 + Subversive Installation Guide</a> &#8211; Installationguide for Spark.
      </p>
    </li>
  </ul>