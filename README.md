# Meme Studio

An android app which lets you fetch latest memes and share among friends.

Used Volley library in Android for the API call.

Used Glide for image loading framework.

### Glide Overview

Glide is a fast and efficient open source media management and image loading 
framework for Android that wraps media decoding, memory and disk caching, and 
resource pooling into a simple and easy to use interface.Glide supports fetching, 
decoding, and displaying video stills, images, and animated GIFs. Glide includes 
a flexible API that allows developers to plug in to almost any network stack.

## Use Gradle

```
repositories {
  google()
  mavenCentral()
}

dependencies {
  implementation 'com.github.bumptech.glide:glide:4.13.2'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.13.2'
}
```
#### For more information on Glide
<https://github.com/bumptech/glide>

### Volley Overview

Volley is an HTTP library that makes networking for Android apps easier and most importantly, faster.

## Send a Simple Request

To use Volley, you must add 
the <android.permission.INTERNET> permission to
your app’s manifest. Without this, your app won’t be able
to connect to the network.

Add Volley to your project.To add the following dependency to your app’s build.gradle file:

```
dependencies {
    implementation("com.android.volley:volley:1.2.1")
}
```

## Use newRequestQueue

Kolin

```
val textView = findViewById<TextView>(R.id.text)
// ...

// Instantiate the RequestQueue.
val queue = Volley.newRequestQueue(this)
val url = "https://www.google.com"

// Request a string response from the provided URL.
val stringRequest = StringRequest(Request.Method.GET, url,
        Response.Listener<String> { response ->
            // Display the first 500 characters of the response string.
            textView.text = "Response is: ${response.substring(0, 500)}"
        },
        Response.ErrorListener { textView.text = "That didn't work!" })

// Add the request to the RequestQueue.
queue.add(stringRequest)
```

Java

```
final TextView textView = (TextView) findViewById(R.id.text);
// ...

// Instantiate the RequestQueue.
RequestQueue queue = Volley.newRequestQueue(this);
String url = "https://www.google.com";

// Request a string response from the provided URL.
StringRequest stringRequest = new StringRequest(Request.Method.GET, url,
            new Response.Listener<String>() {
    @Override
    public void onResponse(String response) {
        // Display the first 500 characters of the response string.
        textView.setText("Response is: " + response.substring(0,500));
    }
}, new Response.ErrorListener() {
    @Override
    public void onErrorResponse(VolleyError error) {
        textView.setText("That didn't work!");
    }
});

// Add the request to the RequestQueue.
queue.add(stringRequest);
```

# Use a singleton pattern

Kotlin

```
class MySingleton constructor(context: Context) {
    companion object {
        @Volatile
        private var INSTANCE: MySingleton? = null
        fun getInstance(context: Context) =
            INSTANCE ?: synchronized(this) {
                INSTANCE ?: MySingleton(context).also {
                    INSTANCE = it
                }
            }
    }
    val imageLoader: ImageLoader by lazy {
        ImageLoader(requestQueue,
                object : ImageLoader.ImageCache {
                    private val cache = LruCache<String, Bitmap>(20)
                    override fun getBitmap(url: String): Bitmap {
                        return cache.get(url)
                    }
                    override fun putBitmap(url: String, bitmap: Bitmap) {
                        cache.put(url, bitmap)
                    }
                })
    }
    val requestQueue: RequestQueue by lazy {
        // applicationContext is key, it keeps you from leaking the
        // Activity or BroadcastReceiver if someone passes one in.
        Volley.newRequestQueue(context.applicationContext)
    }
    fun <T> addToRequestQueue(req: Request<T>) {
        requestQueue.add(req)
    }
}
```

Java

```
public class MySingleton {
    private static MySingleton instance;
    private RequestQueue requestQueue;
    private ImageLoader imageLoader;
    private static Context ctx;

    private MySingleton(Context context) {
        ctx = context;
        requestQueue = getRequestQueue();

        imageLoader = new ImageLoader(requestQueue,
                new ImageLoader.ImageCache() {
            private final LruCache<String, Bitmap>
                    cache = new LruCache<String, Bitmap>(20);

            @Override
            public Bitmap getBitmap(String url) {
                return cache.get(url);
            }

            @Override
            public void putBitmap(String url, Bitmap bitmap) {
                cache.put(url, bitmap);
            }
        });
    }

    public static synchronized MySingleton getInstance(Context context) {
        if (instance == null) {
            instance = new MySingleton(context);
        }
        return instance;
    }

    public RequestQueue getRequestQueue() {
        if (requestQueue == null) {
            // getApplicationContext() is key, it keeps you from leaking the
            // Activity or BroadcastReceiver if someone passes one in.
            requestQueue = Volley.newRequestQueue(ctx.getApplicationContext());
        }
        return requestQueue;
    }

    public <T> void addToRequestQueue(Request<T> req) {
        getRequestQueue().add(req);
    }

    public ImageLoader getImageLoader() {
        return imageLoader;
    }
}
```

### RequestQueue operations using the singleton class

Kotlin

```
MySingleton.getInstance(this).addToRequestQueue(stringRequest)
```

Java

```
MySingleton.getInstance(this).addToRequestQueue(stringRequest);
```

### For more information

<https://google.github.io/volley/requestqueue.html>


# Link to the API

<https://meme-api.herokuapp.com/gimme>


# Screenshots

![WhatsApp Image 2022-09-27 at 10 59 57 PM](https://user-images.githubusercontent.com/91545371/192615228-1ebdc23d-975b-41f8-9fa3-eb9750773cc7.jpeg)


# Video 


https://user-images.githubusercontent.com/91545371/192615441-c26b729f-95a7-4b25-aef8-442fcda6edc4.mp4


# Developer

Reach out to me at 
<https://www.linkedin.com/in/samik-pandit-417a4521b>

<panditsamik25@gmail.com>

