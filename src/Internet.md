Retrofit
public class RetrofitClient {
    static ApiService getService(){
        OkHttpClient.Builder httpClient = new OkHttpClient.Builder();
        Retrofit.Builder builder = new Retrofit.Builder()
                .baseUrl("http://127.0.0.1:5000/")
                .addConverterFactory(GsonConverterFactory.create());

        Retrofit retrofit = builder
                .client(httpClient.build())
                .build();

        return retrofit.create(ApiService.class);
    }
}
RetrofitClient.getService().fetchData(jsonObject).enqueue(new Callback<ResponseClass>() {
    @Override
    public void onResponse(Call<ResponseClass> call, Response<ResponseClass> response) {

    }

    @Override
    public void onFailure(Call<ResponseClass> call, Throwable t) {

    }
});

Moshi - JSON

Chuck - inspector