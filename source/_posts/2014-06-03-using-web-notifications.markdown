---
layout: post
title: "Using Web Notifications"
date: 2014-06-03 16:46:58 +0800
comments: true
categories: HTML5
tags: Notifications
---
<div id="wiki-content" class="column-main wiki-column text-content">
    <!-- just the article content -->
    <article id="wikiArticle">
        <h2 id="Summary">Summary</h2>
        <p>The Web Notifications API allows a web page to send notifications that are displayed outside the page at the system level. This allows web apps to send information to a user even if the application is idle. One of the main obvious use cases is a webmail application that would notify the user each time a new e-mail is received, even if the user is doing something else with another application.</p>
        <p>To display notification you need to first request the appropriate permission and then instantiate a
            <a href="/en-US/docs/Web/API/Notification" title="The Notification object is used to configure and display desktop notifications to the user."><code>Notification</code></a>
            object:</p>
        <pre class="brush: js  language-js" data-number=""><code class=" language-js">Notification<span class="token punctuation">.</span><span class="token function">requestPermission<span class="token punctuation">(</span></span> <span class="token keyword">function</span><span class="token punctuation">(</span>status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log<span class="token punctuation">(</span></span>status<span class="token punctuation">)</span><span class="token punctuation">;</span><span class="token comment" spellcheck="true"> // notifications will only be displayed if "granted"
</span>  <span class="token keyword">var</span> n <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Notification</span><span class="token punctuation">(</span><span class="token string">"title"</span><span class="token punctuation">,</span> <span class="token punctuation">{</span>body<span class="token punctuation">:</span> <span class="token string">"notification body"</span><span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span><span class="token comment" spellcheck="true"> // this also shows the notification
</span><span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span><div class="line-number" data-start="1" style="top: 0px;"></div><div class="line-number" data-start="2" style="top: 19px;"></div><div class="line-number" data-start="3" style="top: 38px;"></div><div class="line-number" data-start="4" style="top: 57px;"></div></code></pre>
        <h2 id="Requesting_permission">Requesting permission</h2>
        <h3 id="Web_content">Web content</h3>
        <p>Before an app is able to send a notification, the user must grant the application the right to do so. This is a common requirement when an API tries to interact with something outside a web page. This guarantees to avoid notification "spam" for the well-being of the user.</p>
        <p>An application that needs to send a notification can check the current permission status thanks to the
            <a href="/en-US/docs/Web/API/Notification.permission" title="The permission property indicates the current permission granted by the user about web notification for the current application."><code>Notification.permission</code></a>read only property. It can have one of three possible values:</p>
        <ul>
            <li>
                <code>default</code>: the user didn't give any permission yet (and therefore no notification will be displayed to the user).</li>
            <li>
                <code>granted</code>: the user explicitly accepted to be notified by the application.</li>
            <li>
                <code>denied</code>: the user explicitly refused to be notified by the application.</li>
        </ul>
        <div class="note">
            <p>
                <strong>Note:</strong>Chrome and Safari do not implement the
                <code>permission</code>property yet.</p>
        </div>
        <p>If the permission is not granted, the application has to use the<a href="/en-US/docs/Web/API/Notification.requestPermission" title="The requestPermission static method is used to ask the user for his permission to display a Notification to him."><code>Notification.requestPermission()</code></a>method to let the user make a choice. This method accepts a callback function that receives the permission chosen by the user in order to react to it.</p>
        <p>It's a common practice to ask for the permission at the initialization of the app:</p>
        <pre class="brush: js  language-js" data-number=""><code class=" language-js">window<span class="token punctuation">.</span><span class="token function">addEventListener<span class="token punctuation">(</span></span><span class="token string">'load'</span><span class="token punctuation">,</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  Notification<span class="token punctuation">.</span><span class="token function">requestPermission<span class="token punctuation">(</span></span><span class="token keyword">function</span> <span class="token punctuation">(</span>status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
   <span class="token comment" spellcheck="true"> // This allows to use Notification.permission with Chrome/Safari
</span>    <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification<span class="token punctuation">.</span>permission <span class="token operator">!</span><span class="token operator">==</span> status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
      Notification<span class="token punctuation">.</span>permission <span class="token operator">=</span> status<span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span><div class="line-number" data-start="1" style="top: 0px;"></div><div class="line-number" data-start="2" style="top: 19px;"></div><div class="line-number" data-start="3" style="top: 38px;"></div><div class="line-number" data-start="4" style="top: 57px;"></div><div class="line-number" data-start="5" style="top: 76px;"></div><div class="line-number" data-start="6" style="top: 95px;"></div><div class="line-number" data-start="7" style="top: 114px;"></div><div class="line-number" data-start="8" style="top: 133px;"></div></code></pre>
        <div class="note">
            <p>
                <strong>Note:</strong>Chrome does not allows to call
                <a href="/en-US/docs/Web/API/Notification.requestPermission" title="The requestPermission static method is used to ask the user for his permission to display a Notification to him."><code>Notification.requestPermission()</code></a>on the<code>load</code>event (see <a class="external external-icon" href="https://code.google.com/p/chromium/issues/detail?id=274284" title="https://code.google.com/p/chromium/issues/detail?id=274284">issue 274284</a>).</p>
        </div>
        <h3 id="Installed_application">Installed application</h3>
        <p>When an application is installed, it's possible to avoid to prompt the user for permission by adding the permission directly within the <a href="/en-US/docs/Web/Apps/Manifest" title="/en-US/docs/Web/Apps/Manifest">application manifest</a>:</p>
        <pre class="brush: json  language-json" data-number=""><code class=" language-json"><span class="token key">"permissions":</span> <span class="token punctuation">{</span>
  <span class="token key">"desktop-notification":</span> <span class="token punctuation">{</span>
    <span class="token key">"description":</span> <span class="token string">"Allows to display notifications on the user's desktop."</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span><div class="line-number" data-start="1" style="top: 0px;"></div><div class="line-number" data-start="2" style="top: 19px;"></div><div class="line-number" data-start="3" style="top: 38px;"></div><div class="line-number" data-start="4" style="top: 57px;"></div><div class="line-number" data-start="5" style="top: 76px;"></div></code></pre>
        <h2 id="Creating_a_notification">Creating a notification</h2>
        <p>Creating a notification is simply done using the
            <a href="/en-US/docs/Web/API/Notification" title="The Notification object is used to configure and display desktop notifications to the user."><code>Notification</code></a>constructor. This constructor expects a title to display within the notification and some options to enhance the notification such as an
            <a href="/en-US/docs/Web/API/Notification.icon" title="The icon property indicates the URL of the icon to use with the notification."><code>icon</code></a>or a text
            <a href="/en-US/docs/Web/API/Notification.body" title="The body property represents the text content of the body of the notification."><code>body</code></a>.</p>
        <p>A notification is displayed as soon as possible when instantiated. To track the current state of a notification, four events are triggered at the
            <a href="/en-US/docs/Web/API/Notification" title="The Notification object is used to configure and display desktop notifications to the user."><code>Notification</code></a>instance level:</p>
        <ul>
            <li>
                <code><a href="/en-US/docs/Web/Reference/Events/show" title="/en-US/docs/Web/Reference/Events/show">show</a></code>: triggered when the notification is displayed to the user.</li>
            <li>
                <code><a href="/en-US/docs/Web/Reference/Events/click" title="/en-US/docs/Web/Reference/Events/click">click</a>
                </code>: triggered when the user clicks on the notification.</li>
            <li>
                <code><a class="new" href="/en-US/docs/Web/Reference/Events/close" title="/en-US/docs/Web/Reference/Events/close">close</a>
                </code>: triggered when the notification is closed.</li>
            <li>
                <code><a href="/en-US/docs/Web/Reference/Events/error" title="/en-US/docs/Web/Reference/Events/error">error</a>
                </code>: triggered when something wrong happens with the notification (mostly when something prevents the notification from being displayed)</li>
        </ul>
        <p>Those events can be tracked using the event handlers
            <a href="/en-US/docs/Web/API/Notification.onshow" title="Specifies an event listener to receive show events. These events occur when a Notification is displayed."><code>onshow</code>
            </a>,
            <a href="/en-US/docs/Web/API/Notification.onclick" title="Specifies an event listener to receive click events. These events occur when the user clicks on a displayed Notification."><code>onclick</code>
            </a>,
            <a href="/en-US/docs/Web/API/Notification.onclose" title="Specifies an event listener to receive close events. These events occur when a Notification is closed."><code>onclose</code>
            </a>, or
            <a href="/en-US/docs/Web/API/Notification.onerror" title="Specifies an event listener to receive error events. These events occur when something goes wrong with a Notification (in many cases an error prevented the notification from being displayed)."><code>onerror</code>
            </a>. Because
            <a href="/en-US/docs/Web/API/Notification" title="The Notification object is used to configure and display desktop notifications to the user."><code>Notification</code>
            </a>also inherits from
            <a href="/en-US/docs/Web/API/EventTarget" title="EventTarget is a DOM interface implemented by objects that can receive DOM events and have listeners for them."><code>EventTarget</code>
            </a>, it's possible to use the
            <a href="/en-US/docs/Web/API/EventTarget.addEventListener" title="The EventTarget.addEventListener() method registers the specified listener on the EventTarget it's called on. The event target may be an Element in a document, the Document itself, a Window, or any other object that supports events (such as XMLHttpRequest)."><code>addEventListener()</code>
            </a>method.</p>
        <div class="note">
            <p>
                <strong>Note:</strong>Firefox and Safari close the notifications automatically after a few moments, e.g. 4&nbsp; seconds.</p>
            <p>This can also be done at the web application level using the
                <a href="/en-US/docs/Web/API/Notification.close" title="The close method is used to close a Notification that has been displayed."><code>Notification.close()</code></a>method, for example with the following code:</p>
            <pre class="brush: js  language-js" data-number=""><code class=" language-js"><span class="token keyword">var</span> n <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Notification</span><span class="token punctuation">(</span><span class="token string">"Hi!"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
n<span class="token punctuation">.</span>onshow <span class="token operator">=</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> 
  <span class="token function">setTimeout<span class="token punctuation">(</span></span>n<span class="token punctuation">.</span>close<span class="token punctuation">,</span> <span class="token number">5000</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
<span class="token punctuation">}</span><div class="line-number" data-start="1" style="top: 0px;"></div><div class="line-number" data-start="2" style="top: 19px;"></div><div class="line-number" data-start="3" style="top: 38px;"></div><div class="line-number" data-start="4" style="top: 57px;"></div></code></pre>
            <p>When you receive a "close" event, there is no guarantee that it's the user who closed the notification. This is in line with the specification, which states: "When a notification is closed, either by the underlying notifications platform or by the user, the close steps for it must be run."</p>
        </div>
        <h3 id="Simple_example">Simple example</h3>
        <p>Assume the following basic HTML:</p>
        <pre class="brush: html  language-html" data-number=""><code class=" language-html"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>button</span><span class="token punctuation">&gt;</span></span>Notify me!<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>button</span><span class="token punctuation">&gt;</span></span><div class="line-number" data-start="1" style="top: 0px;"></div></code></pre>
        <p>It's possible to handle notifications this way:</p>
        <pre class="brush: js  language-js" data-number=""><code class=" language-js">window<span class="token punctuation">.</span><span class="token function">addEventListener<span class="token punctuation">(</span></span><span class="token string">'load'</span><span class="token punctuation">,</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
 <span class="token comment" spellcheck="true"> // At first, let's check if we have permission for notification
</span> <span class="token comment" spellcheck="true"> // If not, let's ask for it
</span>  <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification <span class="token operator">&amp;&amp;</span> Notification<span class="token punctuation">.</span>permission <span class="token operator">!</span><span class="token operator">==</span> <span class="token string">"granted"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    Notification<span class="token punctuation">.</span><span class="token function">requestPermission<span class="token punctuation">(</span></span><span class="token keyword">function</span> <span class="token punctuation">(</span>status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
      <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification<span class="token punctuation">.</span>permission <span class="token operator">!</span><span class="token operator">==</span> status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        Notification<span class="token punctuation">.</span>permission <span class="token operator">=</span> status<span class="token punctuation">;</span>
      <span class="token punctuation">}</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token keyword">var</span> button <span class="token operator">=</span> document<span class="token punctuation">.</span><span class="token function">getElementsByTagName<span class="token punctuation">(</span></span><span class="token string">'button'</span><span class="token punctuation">)</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>

  button<span class="token punctuation">.</span><span class="token function">addEventListener<span class="token punctuation">(</span></span><span class="token string">'click'</span><span class="token punctuation">,</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
   <span class="token comment" spellcheck="true"> // If the user agreed to get notified
</span>    <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification <span class="token operator">&amp;&amp;</span> Notification<span class="token punctuation">.</span>permission <span class="token operator">==</span><span class="token operator">=</span> <span class="token string">"granted"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      <span class="token keyword">var</span> n <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Notification</span><span class="token punctuation">(</span><span class="token string">"Hi!"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

   <span class="token comment" spellcheck="true"> // If the user hasn't told if he wants to be notified or not
</span>   <span class="token comment" spellcheck="true"> // Note: because of Chrome, we are not sure the permission property
</span>   <span class="token comment" spellcheck="true"> // is set, therefore it's unsafe to check for the "default" value.
</span>    <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification <span class="token operator">&amp;&amp;</span> Notification<span class="token punctuation">.</span>permission <span class="token operator">!</span><span class="token operator">==</span> <span class="token string">"denied"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      Notification<span class="token punctuation">.</span><span class="token function">requestPermission<span class="token punctuation">(</span></span><span class="token keyword">function</span> <span class="token punctuation">(</span>status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification<span class="token punctuation">.</span>permission <span class="token operator">!</span><span class="token operator">==</span> status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
          Notification<span class="token punctuation">.</span>permission <span class="token operator">=</span> status<span class="token punctuation">;</span>
        <span class="token punctuation">}</span>

       <span class="token comment" spellcheck="true"> // If the user said okay
</span>        <span class="token keyword">if</span> <span class="token punctuation">(</span>status <span class="token operator">==</span><span class="token operator">=</span> <span class="token string">"granted"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
          <span class="token keyword">var</span> n <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Notification</span><span class="token punctuation">(</span><span class="token string">"Hi!"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span>

       <span class="token comment" spellcheck="true"> // Otherwise, we can fallback to a regular modal alert
</span>        <span class="token keyword">else</span> <span class="token punctuation">{</span>
          <span class="token function">alert<span class="token punctuation">(</span></span><span class="token string">"Hi!"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span>
      <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

   <span class="token comment" spellcheck="true"> // If the user refuses to get notified
</span>    <span class="token keyword">else</span> <span class="token punctuation">{</span>
     <span class="token comment" spellcheck="true"> // We can fallback to a regular modal alert
</span>      <span class="token function">alert<span class="token punctuation">(</span></span><span class="token string">"Hi!"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span><div class="line-number" data-start="1" style="top: 0px;"></div><div class="line-number" data-start="2" style="top: 19px;"></div><div class="line-number" data-start="3" style="top: 38px;"></div><div class="line-number" data-start="4" style="top: 57px;"></div><div class="line-number" data-start="5" style="top: 76px;"></div><div class="line-number" data-start="6" style="top: 95px;"></div><div class="line-number" data-start="7" style="top: 114px;"></div><div class="line-number" data-start="8" style="top: 133px;"></div><div class="line-number" data-start="9" style="top: 152px;"></div><div class="line-number" data-start="10" style="top: 171px;"></div><div class="line-number" data-start="11" style="top: 190px;"></div><div class="line-number" data-start="12" style="top: 209px;"></div><div class="line-number" data-start="13" style="top: 228px;"></div><div class="line-number" data-start="14" style="top: 247px;"></div><div class="line-number" data-start="15" style="top: 266px;"></div><div class="line-number" data-start="16" style="top: 285px;"></div><div class="line-number" data-start="17" style="top: 304px;"></div><div class="line-number" data-start="18" style="top: 323px;"></div><div class="line-number" data-start="19" style="top: 342px;"></div><div class="line-number" data-start="20" style="top: 361px;"></div><div class="line-number" data-start="21" style="top: 380px;"></div><div class="line-number" data-start="22" style="top: 399px;"></div><div class="line-number" data-start="23" style="top: 418px;"></div><div class="line-number" data-start="24" style="top: 437px;"></div><div class="line-number" data-start="25" style="top: 456px;"></div><div class="line-number" data-start="26" style="top: 475px;"></div><div class="line-number" data-start="27" style="top: 494px;"></div><div class="line-number" data-start="28" style="top: 513px;"></div><div class="line-number" data-start="29" style="top: 532px;"></div><div class="line-number" data-start="30" style="top: 551px;"></div><div class="line-number" data-start="31" style="top: 570px;"></div><div class="line-number" data-start="32" style="top: 589px;"></div><div class="line-number" data-start="33" style="top: 608px;"></div><div class="line-number" data-start="34" style="top: 627px;"></div><div class="line-number" data-start="35" style="top: 646px;"></div><div class="line-number" data-start="36" style="top: 665px;"></div><div class="line-number" data-start="37" style="top: 684px;"></div><div class="line-number" data-start="38" style="top: 703px;"></div><div class="line-number" data-start="39" style="top: 722px;"></div><div class="line-number" data-start="40" style="top: 741px;"></div><div class="line-number" data-start="41" style="top: 760px;"></div><div class="line-number" data-start="42" style="top: 779px;"></div><div class="line-number" data-start="43" style="top: 798px;"></div><div class="line-number" data-start="44" style="top: 817px;"></div><div class="line-number" data-start="45" style="top: 836px;"></div><div class="line-number" data-start="46" style="top: 855px;"></div><div class="line-number" data-start="47" style="top: 874px;"></div></code></pre>
        <p>And the live result:</p>
        <p>
            <iframe class="live-sample-frame" frameborder="0" height="30" src="https://mdn.mozillademos.org/en-US/docs/WebAPI/Using_Web_Notifications$samples/Simple_example?revision=561527" width="100%"></iframe>
        </p>
        <h2 id="Dealing_with_repeated_notifications">Dealing with repeated notifications</h2>
        <p>In some cases it can be painful for the user to send him a high number of notifications--for example, if an application for instant messaging can notify a user for each incoming message. To avoid bloating the user desktop with hundreds of unnecessary notifications, it's possible to take over the queue of pending notifications.</p>
        <p>To do this, it's possible to add a tag to any new notification. If a notification already has the same tag and has not been displayed yet, the new notification will replace that previous notification. If the notification with the same tag has been already displayed, the previous notification is closed and the new one is displayed.</p>
        <h3 id="Tag_example">Tag example</h3>
        <p>Assume the following basic HTML:</p>
        <pre class="brush: html  language-html" data-number=""><code class=" language-html"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>button</span><span class="token punctuation">&gt;</span></span>Notify me!<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>button</span><span class="token punctuation">&gt;</span></span><div class="line-number" data-start="1" style="top: 0px;"></div></code></pre>
        <p>It's possible to handle multiple notifications this way:</p>
        <pre class="brush: js  language-js" data-number=""><code class=" language-js">window<span class="token punctuation">.</span><span class="token function">addEventListener<span class="token punctuation">(</span></span><span class="token string">'load'</span><span class="token punctuation">,</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
 <span class="token comment" spellcheck="true"> // At first, let's check if we have permission for notification
</span> <span class="token comment" spellcheck="true"> // If not, let's ask for it
</span>  <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification <span class="token operator">&amp;&amp;</span> Notification<span class="token punctuation">.</span>permission <span class="token operator">!</span><span class="token operator">==</span> <span class="token string">"granted"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    Notification<span class="token punctuation">.</span><span class="token function">requestPermission<span class="token punctuation">(</span></span><span class="token keyword">function</span> <span class="token punctuation">(</span>status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
      <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification<span class="token punctuation">.</span>permission <span class="token operator">!</span><span class="token operator">==</span> status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        Notification<span class="token punctuation">.</span>permission <span class="token operator">=</span> status<span class="token punctuation">;</span>
      <span class="token punctuation">}</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token keyword">var</span> button <span class="token operator">=</span> document<span class="token punctuation">.</span><span class="token function">getElementsByTagName<span class="token punctuation">(</span></span><span class="token string">'button'</span><span class="token punctuation">)</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>

  button<span class="token punctuation">.</span><span class="token function">addEventListener<span class="token punctuation">(</span></span><span class="token string">'click'</span><span class="token punctuation">,</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
   <span class="token comment" spellcheck="true"> // If the user agreed to get notified
</span>   <span class="token comment" spellcheck="true"> // Let's try to send ten notifications
</span>    <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification <span class="token operator">&amp;&amp;</span> Notification<span class="token punctuation">.</span>permission <span class="token operator">==</span><span class="token operator">=</span> <span class="token string">"granted"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">var</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token number">10</span><span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
       <span class="token comment" spellcheck="true"> // Thanks to the tag, we should only see the "Hi! 9" notification
</span>        <span class="token keyword">var</span> n <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Notification</span><span class="token punctuation">(</span><span class="token string">"Hi! "</span> <span class="token operator">+</span> i<span class="token punctuation">,</span> <span class="token punctuation">{</span>tag<span class="token punctuation">:</span> <span class="token string">'soManyNotification'</span><span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token punctuation">}</span>
    <span class="token punctuation">}</span>

   <span class="token comment" spellcheck="true"> // If the user hasn't told if he wants to be notified or not
</span>   <span class="token comment" spellcheck="true"> // Note: because of Chrome, we are not sure the permission property
</span>   <span class="token comment" spellcheck="true"> // is set, therefore it's unsafe to check for the "default" value.
</span>    <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification <span class="token operator">&amp;&amp;</span> Notification<span class="token punctuation">.</span>permission <span class="token operator">!</span><span class="token operator">==</span> <span class="token string">"denied"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      Notification<span class="token punctuation">.</span><span class="token function">requestPermission<span class="token punctuation">(</span></span><span class="token keyword">function</span> <span class="token punctuation">(</span>status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">if</span> <span class="token punctuation">(</span>Notification<span class="token punctuation">.</span>permission <span class="token operator">!</span><span class="token operator">==</span> status<span class="token punctuation">)</span> <span class="token punctuation">{</span>
          Notification<span class="token punctuation">.</span>permission <span class="token operator">=</span> status<span class="token punctuation">;</span>
        <span class="token punctuation">}</span>

       <span class="token comment" spellcheck="true"> // If the user said okay
</span>        <span class="token keyword">if</span> <span class="token punctuation">(</span>status <span class="token operator">==</span><span class="token operator">=</span> <span class="token string">"granted"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
          <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">var</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token number">10</span><span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
           <span class="token comment" spellcheck="true"> // Thanks to the tag, we should only see the "Hi! 9" notification
</span>            <span class="token keyword">var</span> n <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Notification</span><span class="token punctuation">(</span><span class="token string">"Hi! "</span> <span class="token operator">+</span> i<span class="token punctuation">,</span> <span class="token punctuation">{</span>tag<span class="token punctuation">:</span> <span class="token string">'soManyNotification'</span><span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
          <span class="token punctuation">}</span>
        <span class="token punctuation">}</span>

       <span class="token comment" spellcheck="true"> // Otherwise, we can fallback to a regular modal alert
</span>        <span class="token keyword">else</span> <span class="token punctuation">{</span>
          <span class="token function">alert<span class="token punctuation">(</span></span><span class="token string">"Hi!"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span>
      <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

   <span class="token comment" spellcheck="true"> // If the user refuses to get notified
</span>    <span class="token keyword">else</span> <span class="token punctuation">{</span>
     <span class="token comment" spellcheck="true"> // We can fallback to a regular modal alert
</span>      <span class="token function">alert<span class="token punctuation">(</span></span><span class="token string">"Hi!"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span><div class="line-number" data-start="1" style="top: 0px;"></div><div class="line-number" data-start="2" style="top: 19px;"></div><div class="line-number" data-start="3" style="top: 38px;"></div><div class="line-number" data-start="4" style="top: 57px;"></div><div class="line-number" data-start="5" style="top: 76px;"></div><div class="line-number" data-start="6" style="top: 95px;"></div><div class="line-number" data-start="7" style="top: 114px;"></div><div class="line-number" data-start="8" style="top: 133px;"></div><div class="line-number" data-start="9" style="top: 152px;"></div><div class="line-number" data-start="10" style="top: 171px;"></div><div class="line-number" data-start="11" style="top: 190px;"></div><div class="line-number" data-start="12" style="top: 209px;"></div><div class="line-number" data-start="13" style="top: 228px;"></div><div class="line-number" data-start="14" style="top: 247px;"></div><div class="line-number" data-start="15" style="top: 266px;"></div><div class="line-number" data-start="16" style="top: 285px;"></div><div class="line-number" data-start="17" style="top: 304px;"></div><div class="line-number" data-start="18" style="top: 323px;"></div><div class="line-number" data-start="19" style="top: 342px;"></div><div class="line-number" data-start="20" style="top: 361px;"></div><div class="line-number" data-start="21" style="top: 380px;"></div><div class="line-number" data-start="22" style="top: 399px;"></div><div class="line-number" data-start="23" style="top: 418px;"></div><div class="line-number" data-start="24" style="top: 437px;"></div><div class="line-number" data-start="25" style="top: 456px;"></div><div class="line-number" data-start="26" style="top: 475px;"></div><div class="line-number" data-start="27" style="top: 494px;"></div><div class="line-number" data-start="28" style="top: 513px;"></div><div class="line-number" data-start="29" style="top: 532px;"></div><div class="line-number" data-start="30" style="top: 551px;"></div><div class="line-number" data-start="31" style="top: 570px;"></div><div class="line-number" data-start="32" style="top: 589px;"></div><div class="line-number" data-start="33" style="top: 608px;"></div><div class="line-number" data-start="34" style="top: 627px;"></div><div class="line-number" data-start="35" style="top: 646px;"></div><div class="line-number" data-start="36" style="top: 665px;"></div><div class="line-number" data-start="37" style="top: 684px;"></div><div class="line-number" data-start="38" style="top: 703px;"></div><div class="line-number" data-start="39" style="top: 722px;"></div><div class="line-number" data-start="40" style="top: 741px;"></div><div class="line-number" data-start="41" style="top: 760px;"></div><div class="line-number" data-start="42" style="top: 779px;"></div><div class="line-number" data-start="43" style="top: 798px;"></div><div class="line-number" data-start="44" style="top: 817px;"></div><div class="line-number" data-start="45" style="top: 836px;"></div><div class="line-number" data-start="46" style="top: 855px;"></div><div class="line-number" data-start="47" style="top: 874px;"></div><div class="line-number" data-start="48" style="top: 893px;"></div><div class="line-number" data-start="49" style="top: 912px;"></div><div class="line-number" data-start="50" style="top: 931px;"></div><div class="line-number" data-start="51" style="top: 950px;"></div><div class="line-number" data-start="52" style="top: 969px;"></div><div class="line-number" data-start="53" style="top: 988px;"></div><div class="line-number" data-start="54" style="top: 1007px;"></div></code></pre>
        <p>And the live result:</p>
        <p>
            <iframe class="live-sample-frame" frameborder="0" height="30" src="https://mdn.mozillademos.org/en-US/docs/WebAPI/Using_Web_Notifications$samples/Tag_example?revision=561527" width="100%"></iframe>
        </p>
        <h2 id="Specifications">Specifications</h2>
        <table class="standard-table">
            <thead>
                <tr>
                    <th scope="col">Specification</th>
                    <th scope="col">Status</th>
                    <th scope="col">Comment</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><a class="external external-icon" href="https://dvcs.w3.org/hg/notifications/raw-file/tip/Overview.html" hreflang="en" lang="en" title="The 'Web Notifications' specification">Web Notifications</a>
                    </td>
                    <td>
                        <span class="spec-WD">Working Draft</span>
                    </td>
                    <td>Initial specification.</td>
                </tr>
            </tbody>
        </table>
        <h2 id="Browser_compatibility">Browser compatibility</h2>
        <p></p>
        <p></p>
        <div class="htab">
            <a id="AutoCompatibilityTable" name="AutoCompatibilityTable">
            </a>
            <ul>
                <a id="AutoCompatibilityTable" name="AutoCompatibilityTable"></a>
                <li class="selected">
                    <a id="AutoCompatibilityTable" name="AutoCompatibilityTable"></a><a>Desktop</a>
                </li>
                <li><a>Mobile</a>
                </li>
            </ul>
            <div id="compat-desktop" style="display: block;">
                <table class="compat-table">
                    <tbody>
                        <tr>
                            <th>Feature</th>
                            <th>Chrome</th>
                            <th>Firefox (Gecko)</th>
                            <th>Internet Explorer</th>
                            <th>Opera</th>
                            <th>Safari</th>
                        </tr>
                        <tr>
                            <td>Basic support</td>
                            <td>5
                                <span class="inlineIndicator prefixBox prefixBoxInline" title="prefix">webkit</span>(see notes)
                                <br>22
                            </td>
                            <td>4.0
                                <span class="inlineIndicator prefixBox prefixBoxInline" title="prefix">moz</span>(see notes)
                                <br>22
                            </td>
                            <td>
                                <span style="color: #f00;">Not&nbsp;supported</span>
                            </td>
                            <td>
                                <span style="color: rgb(255, 153, 0);" title="Compatibility unknown; please update this.">?</span>
                            </td>
                            <td>6 (see notes)</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div id="compat-mobile" style="display: none;">
                <table class="compat-table">
                    <tbody>
                        <tr>
                            <th>Feature</th>
                            <th>Android</th>
                            <th>Firefox Mobile (Gecko)</th>
                            <th>Firefox OS</th>
                            <th>IE Mobile</th>
                            <th>Opera Mobile</th>
                            <th>Safari Mobile</th>
                        </tr>
                        <tr>
                            <td>Basic support</td>
                            <td>
                                <span style="color: rgb(255, 153, 0);" title="Compatibility unknown; please update this.">?</span>
                            </td>
                            <td>4.0
                                <span class="inlineIndicator prefixBox prefixBoxInline" title="prefix">moz</span>(see notes)
                                <br>22
                            </td>
                            <td>1.0.1
                                <span class="inlineIndicator prefixBox prefixBoxInline" title="prefix">moz</span>(see notes)
                                <br>1.2
                            </td>
                            <td>
                                <span style="color: #f00;">Not&nbsp;supported</span>
                            </td>
                            <td>
                                <span style="color: rgb(255, 153, 0);" title="Compatibility unknown; please update this.">?</span>
                            </td>
                            <td>
                                <span style="color: rgb(255, 153, 0);" title="Compatibility unknown; please update this.">?</span>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <p>
        </p>

        <h3 id="Gecko_notes">Gecko notes</h3>
        <ul>
            <li>Prior to Firefox 22 (Firefox OS &lt;1.2), the instantiation of a new notification must be done with the
                <a href="/en-US/docs/Web/API/window.navigator.mozNotification" title="Provides support for creating notification objects, which are used to display desktop notification alerts to the user. Currently, these are only supported on Firefox Mobile and Firefox OS. See Displaying notifications for an example.">
                    <code>navigator.mozNotification</code>
                </a>object through its
                <code>createNotification</code>method.</li>
            <li>Prior to Firefox 22 (Firefox OS &lt;1.2), the Notification was displayed when calling the
                <code>show</code>method and was supporting the
                <code>click</code>and
                <code>close</code>events only.</li>
            <li>Nick Desaulniers has written a <a class="external external-icon" href="https://github.com/nickdesaulniers/fxos-irc/blob/master/js/notification.js">Notification shim</a> to cover both newer and older implementations.</li>
            <li>One particular Firefox OS issue is that you can <a class="external external-icon" href="https://github.com/nickdesaulniers/fxos-irc/blob/0160cf6c3a2b5c9fe33822aaf6bcba3b7e846da9/my.js#L171">pass a path to an icon</a> to use in the notification, but if the app is packaged you cannot use a relative path like
                <code>/my_icon.png</code>. You also can't use
                <code>window.location.origin + "/my_icon.png"</code>because
                <code>window.location.origin</code>is null in packaged apps. The <a href="https://developer.mozilla.org/en-US/Apps/Developing/Manifest#origin">manifest origin field</a> fixes this, but it is only available in Firefox OS 1.1+. A potential solution for supporting Firefox OS &lt;1.1 is to <a class="external external-icon" href="https://github.com/nickdesaulniers/fxos-irc/blob/0160cf6c3a2b5c9fe33822aaf6bcba3b7e846da9/my.js#L168">pass an absolute URL to an externally hosted version of the icon</a>. This is less than ideal as the notification is displayed immediately with the icon missing, then the icon is fetched, but it works on all versions of Firefox OS.</li>
        </ul>
        <h3 id="Chrome_notes">Chrome notes</h3>
        <ul>
            <li>Prior to Chrome 22, the support for notification was following an <a class="external external-icon" href="http://www.chromium.org/developers/design-documents/desktop-notifications/api-specification" title="http://www.chromium.org/developers/design-documents/desktop-notifications/api-specification">old prefixed version of the specification</a> and was using the
                <a class="new" href="/en-US/docs/Web/API/window.navigator.webkitNotifications" title="The documentation about this has not yet been written; please consider contributing!"><code>navigator.webkitNotifications</code>
                </a>object to instantiate a new notification.</li>
            <li>Prior to Chrome 32,
                <a href="/en-US/docs/Web/API/Notification.permission" title="The permission property indicates the current permission granted by the user about web notification for the current application."><code>Notification.permission</code>
                </a>was not supported.</li>
        </ul>
        <h3 id="Safari_notes">Safari notes</h3>
        <ul>
            <li>Safari started supporting notification with Safari 6 but only on Mac OSX 10.8+ (Mountain Lion).</li>
        </ul>
        <p></p>
        <h2 id="See_also">See also</h2>
        <ul>
            <li>
                <a href="/en-US/docs/Web/API/Notification" title="The Notification object is used to configure and display desktop notifications to the user."><code>Notification</code></a>
            </li>
        </ul>
    </article>


    <!-- attachments list -->

    <!-- contributors -->
    <div class="wiki-block contributors">
        <h2 class="offscreen">Document Tags and Contributors</h2>
        <!-- tags if present -->
        <div class="tag-attach-list tags contributor-sub">
            <i aria-hidden="true" class="icon-tags"></i>
            <strong>Tags:</strong>
            <ul class="tag-list">
                <li><a href="/en-US/docs/tag/Notifications">Notifications</a>
                </li>
                <li><a href="/en-US/docs/tag/Guide">Guide</a>
                </li>
                <li><a href="/en-US/docs/tag/Firefox%20OS">Firefox OS</a>
                </li>
                <li><a href="/en-US/docs/tag/B2G">B2G</a>
                </li>
                <li><a href="/en-US/docs/tag/Advanced">Advanced</a>
                </li>
                <li><a href="/en-US/docs/tag/WebAPI">WebAPI</a>
                </li>
                <li><a href="/en-US/docs/tag/DOM">DOM</a>
                </li>
            </ul>
        </div>
        <div class="contributor-sub"> <i aria-hidden="true" class="icon-group"></i>
            <strong>Contributors to this page:</strong> <a href="/en-US/profiles/kscarfone">kscarfone</a>, <a href="/en-US/profiles/SignpostMarv">SignpostMarv</a>, <a href="/en-US/profiles/Nickolay">Nickolay</a>, <a href="/en-US/profiles/claudepache">claudepache</a>, <a href="/en-US/profiles/damianmoore">damianmoore</a>, <a href="/en-US/profiles/BenB">BenB</a>, <a href="/en-US/profiles/gschnieders">gschnieders</a>, <a href="/en-US/profiles/Jeremie">Jeremie</a>, <a href="/en-US/profiles/derekmeyer">derekmeyer</a> 
        </div>
        <div class="contributor-sub">
            <i aria-hidden="true" class="icon-time"></i>
            <strong>Last updated by:</strong>
            <a href="/en-US/profiles/BenB">BenB</a>,
            <time datetime="2014-04-16T12:05:37-07:00">Apr 16, 2014 12:05:37 PM</time>
        </div>
    </div>
</div>
