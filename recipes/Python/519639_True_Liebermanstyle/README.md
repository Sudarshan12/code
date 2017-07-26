## True Lieberman-style delegation in Python. 
Originally published: 2007-05-14 11:14:33 
Last updated: 2007-05-14 11:14:33 
Author: Martin Blais 
 
Proxies are usually implemented as objects that forward method calls to a\n"target" object. This approach has a major problem: forwarding makes the target\nobject the receiver of the method call; this means that calls originating from\nthe body of a method in the target will not go through the proxy (and thus their\nbehavior cannot be modified by the proxy).\n\nFor example, suppose we want a proxy to an instance of Target (shown below)\nthat is "safe", i.e., does not do anything bad like firing missiles. We can\njust define a class that forwards calls to the safe methods, namely\nsend_flowers() and hang_out(). This class can have its own version of\nfire_missiles() that does nothing. Now consider what happens when we call\nthe proxy object's innocent-looking hang_out() method. The call is forwarded\nto the target object, which in turn calls the target object's (not the\nproxy's) fire_missiles() method, and BOOM! (The proxy's version of\nfire_missiles() is not called because forwarding has made the target object\nthe receiver of the new method call.)\n\nBy using delegation, one can implement proxies without the drawbacks of the\nmethod-forwarding approach. This recipe shows how Python's __getattr__\nmethod can be used to implement the kind of delegation present in\nprototype-based languages like Self and Javascript, and how delegation can\nbe used to implement better proxies.