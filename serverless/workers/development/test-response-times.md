
# Testing API Response Time

When configuring an API, you have multiple options available at various price points and resource allocations. You can choose a single option if you prefer to stick to one price point, or you can set a preference order among the pools to allocate your requests accordingly.

![Image](/assets/images/742bf51-image-cff83571196f6b2b13ae24cb5bd7df47.png)

The most cost-effective option for you will depend on your specific use case and your tolerance for task run time. Each situation is unique, so it's worthwhile to conduct some testing to determine not only how long your tasks will take to run but also how much you might expect to pay for each task.

To measure how long a task will take to run, select a single pool type as shown in the image above. Then, send a request to the API using your preferred method. If you're unfamiliar with how to do this or don't have your own method, you can use a free tool like [reqbin.com](https://reqbin.com/) to send an API request to the SubModel servers.

The URLs to use in the API will be displayed on the My APIs screen:

![Image](/assets/images/0d8dd86-image-8a23d98f45eed07fe6180dad5064f4c9.png)

On reqbin.com, enter the Run URL of your API, select POST from the dropdown, and enter your API key provided when you created the key under [Settings](https://www.submodel.ai/console/serverless/user/settings) (if you don't have it saved, you'll need to return to Settings and create a new key). Under Content, you'll also need to provide a basic command (in this example, we've used a Stable Diffusion prompt).

![Image](/assets/images/a9b9cf3-image-553445aee2a6def062ebb7a925453aae.png)

![Image](/assets/images/7744b62-image-3c8c3981552d48e48c54d34527a9abdd.png)

Send the request, and it will provide you with an ID for the request and notify you that it is processing. You can then replace the URL in the request field with the Status address and append the ID to the end of it, then click Send.

![Image](/assets/images/325f2bc-image-d0464eebb7314399317842515da851f6.png)

It will return a Delay Time and an Execution Time, both denoted in milliseconds. The Delay Time should be minimal unless the API process was initiated from a cold start, in which case a significant delay is expected for the first request. The Execution Time indicates how long the GPU took to process the request once it was received. It may be beneficial to run multiple tests to obtain a minimum, maximum, and average run time—five tests should provide an adequate sample size.

![Image](/assets/images/1608d44-image-0ae69f46dc677749ae19ba027efd62c4.png)

You can then switch to a different GPU pool and repeat the process.

Ultimately, the best option for your use case will depend on how long you can afford to let the process run. For more demanding tasks, using a slower GPU may be more cost-effective, albeit with a tradeoff in speed. For simpler tasks, there may be diminishing returns on speed improvements that higher-end GPUs offer. Experiment to find the optimal balance for your scenario.