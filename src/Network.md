thread {
        val json = try {
            URL(url).readText()
        } catch (e: Exception) {
            return@thread
        }
        runOnUiThread { displayOrWhatever(json) }
    }
    
    
    launch {
    
        val jsonStr = URL("url").readText()
    
    }
    If you need to test with plain http don't forget to add to your manifest: android:usesCleartextTraffic="true"

https://gist.github.com/dmitryweiner/1d004458752abce118e5b0ce2891e9da

https://gist.github.com/dmitryweiner/b27b2741ae58a047f587fedeba0c6755