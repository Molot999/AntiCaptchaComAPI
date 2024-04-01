# AntiCaptchaComAPI
Бесплатная C# библиотека для использования anti-captcha.com API
<hr>
<h2>Использование</h2>
      Добавьте ссылку на AntiCaptchaComAPI.dll и подлючите, используя "using AntiCaptchaComAPI"
<hr>
<h2>Пример работы</h2>

            const string clientKey = "dsaf3gfh4esdsgh4j5ergs"; //ClientKey, полученный на сайте.

            byte[] byteImg = File.ReadAllBytes(@"E/captcha.jpg"); //Перекодируем картинку в массив байтов
            string base64Img = Convert.ToBase64String(byteImg); //А из него в формат base64.

            ImageToTextTask newTask = new ImageToTextTask(base64Img); //Для начала необходимо создать объект определенной задачи
            CaptchaTask captchaTask = new CaptchaTask(clientKey, newTask); //И поместить его в объект CaptchaTask, выставив в конструкторе класса необходимые настройки.

           CaptchaTaskStatus taskStatus = Captcha.CreateTask(captchaTask); //Метод CreateTask посылает объект задачи к API и возвращает объект типа CaptchaTaskStatus с информацией об ошибках и порядковом номере задания (если оно было принято).

            while (true) //Помещаем метод GetTaskResult в цикл, чтобы отследить статус выполнения задачи и получить результат, когда оно будет готово.
            {
                CaptchaTaskResult taskResult = Captcha.GetTaskResult(clientKey, taskStatus.taskId, CaptchaType.ImageToText); //После того как задание было успешно отправлено и получен его taskId можно вызывать метод GetTaskResult, в которого кроме вашего ClientKey и taskId необходимоотправить тип задачи, который вы послали.

                ImageToTextTaskResult result = (ImageToTextTaskResult)taskResult.solution; //После того, как статус задания стал "ready" можно получить текст решенной капчи, после создания объекта с нужным типом, используя свойство solution.

                if (taskResult.status == "ready")
                {
                    Console.WriteLine(result.text); //Выводим решение капчи в консоль
                    break;
                }
            }
          
<hr>
