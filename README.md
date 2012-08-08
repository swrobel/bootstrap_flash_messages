# bootstrap_flash_messages

    version 0.0.1
    Robin Brouwer
    45north

Bootstrap alerts and Rails flash messages combined in one easy-to-use gem.


## Installation

You can use this gem by putting the following inside your Gemfile:

    gem "bootstrap_flash_messages"

Now you need flash.en.yml for the flash messages.

    rails g bootstrap_flash_messages:locale

And that's it!


## Changes

Version 0.0.1 changes (8/08/2012):
    
    - Changed redirect_to method
    - Added flash! and flash_now! methods
    - Added flash_messages helper
    - Made a gem out of it


## Usage

You need [Twitter Bootstrap](http://twitter.github.com/bootstrap/components.html#alerts) for the styling and close button. You can still use it without Bootstrap, but you need to style it yourself.

All flash messages are defined inside config/locales/flash.en.yml. They are nested like this:

    en:
      flash_messages:
        controller_name:
          action_name:
            success: "It worked!"
            info: "There's something you need to know..."
            warning: "You should watch out now."
            error: "Oh no! Something went wrong."

You have four keys:
    
    :success
    :info
    :warning
    :error

When you defined the messages it's really easy to set them inside your Controller.

    class PostsController
      def create
        @post = Post.new(params[:post])
        if @post.save
          redirect_to(@post, :flash => :success)
        else
          flash_now!(:error)
          render("new")
        end
      end
    end

You can use the `:flash` parameter inside the `redirect_to` method (note: this gem changes the redirect_to method!) to set the flash messages. You only need to pass the corresponding key and the gem will automatically set `gflash[:success]` to `t("flash_messages.posts.create.success")`. The `:flash` parameter also accepts an Array.

    redirect_to(@post, :flash => [:success, :info])

You can still use a Hash to use the default `redirect_to` behaviour. When you don't want to use the `redirect_to` method to set the flash messages, you can use the `flash!` method.

    flash!(:success, :info)
    redirect_to(@post)

When you want to use `flash.now` you can use the new `flash_now!` method.

    flash_now!(:error, :warning)

You use `flash_now!` in combination with rendering a view instead of redirecting.

Now's the time to show these messages to the user. Inside the layout (or any other view), add the following:

    <div id="flash_messages"><%= flash_messages %></div>

And that's it! To change the flash messages inside a `.js.erb` file, you can do the following:

    $("#flash_messages").html("#{j(flash_messages)}");

The `flash_messages` helper shows a simple Bootstrap alert box. When you want to add a close button you can add the :close option.

    <%= flash_messages(:close) %>

Want to use the `.alert-block` class? Just add :block.

    <%= flash_messages(:close, :block) %>

Want a heading? Add :heading. The headings inside flash.en.yml are used.

    <%= flash_messages(:close, :block, :heading) %>

And that's it!


## Why I created this gem

I created the [gritter gem](https://github.com/RobinBrouwer/gritter) and used it in a lot of projects.
I started using Twitter Bootstrap and really liked the alerts. I loved the way gritter allowed you to set messages
and decided to do the same for the Bootstrap alerts. And this is the result!

## Copyright

Copyright (C) 2012 Robin Brouwer

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.