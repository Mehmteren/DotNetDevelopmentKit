* MasstransitConfiguration
```razor
using MassTransit;

builder.Services.AddMassTransit(configurator =>
{ 
// Burada, mesajları işlemek için 3 tane consumer (tüketici) ekliyoruz.
    configurator.AddConsumer<OrderCompletedEventConsumer>();
    configurator.AddConsumer<OrderFailedEventConsumer>();
    configurator.AddConsumer<CreateOrderCommandConsumer>();

    configurator.UsingRabbitMq((context, _configure) =>
    {
     // RabbitMQ host adresini alıyoruz ve bağlantıyı sağlıyoruz.
        _configure.Host(builder.Configuration["RabbitMQ"]);
        // Order tamamlandığında mesajları dinleyecek olan kuyruğu ayarlıyoruz.
        _configure.ReceiveEndpoint(RabbitMQSettings.Order_OrderCompletedQueue, e =>
            // OrderCompletedEventConsumer sınıfını bu kuyruğa bağladık.
            e.ConfigureConsumer<OrderCompletedEventConsumer>(context));

        _configure.ReceiveEndpoint(RabbitMQSettings.Order_OrderFailedQueue, e =>
            e.ConfigureConsumer<OrderFailedEventConsumer>(context));

        _configure.ReceiveEndpoint(RabbitMQSettings.Order_OrderCreatedQueue, e =>
            e.ConfigureConsumer<CreateOrderCommandConsumer>(context)); 
    });
});
```
* consumer eklenmesi: Mesajları dinleyip işleyen sınıflar burada belirtiliyor. Her bir consumer, belirli bir olay (event) gerçekleştiğinde çalışacak.
* RabbitMQ host ayarı: RabbitMQ'nin bağlanacağı host adresi yapılandırma dosyasından alınıyor.
* ReceiveEndpoint ayarları: Her bir ReceiveEndpoint, belirli bir kuyruğa mesajları dinleyip, hangi consumer sınıfının bu mesajları işleyeceğini belirtiyor.






