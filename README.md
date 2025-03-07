<h2>
  &#128190; Копирование текста
</h2>

<pre>
  const copyLink = () => {
    const node = document.querySelectorAll(".copy-link");

    if(!node) return;

    const handleCLick = (btn) => {
        const text = btn.getAttribute("data-link-copy").trim();
        if (text) {
            navigator.clipboard.writeText(text)
            .then(() => {
                btn.classList.add('is-copy');

                setTimeout(()=> {
                    btn.classList.remove('is-copy');
                },1000);
            })
            .catch(err => {
                console.error('Something went wrong', err);
        })
    }
    }
    
    node.forEach(btn=> {
        btn.addEventListener("click", ()=>handleCLick(btn));
    });
}

export default copyLink
</pre>

<h2>
 &#127490; Модальные окна
</h2>

<pre>
  const modal = () => {
    // массив всех модалок на странице
    // 1. Кнопка/Кнопки открытия
    // 2. Модальное окно
    // 3. Кнопка скрытия
    // 4. Активный класс
    // Кнопку/Кнопки открытия задавать через querySelectorAll
    const node = [
        [
            document.querySelectorAll('.header__burger'), 
            document.querySelector(".big-menu"), 
            document.querySelector(".big-menu__btn-close"), 
            "is-open",
        ],
        [
            document.querySelectorAll(".btn-callback"),
            document.querySelector(".callbakc-modal"),
            document.querySelector(".callbakc-modal__btn-close"),
            "is-open"
        ],
        [
            document.querySelectorAll(".sucsses-modal-open"),
            document.querySelector(".sucsses-modal"),
            document.querySelector(".sucsses-modal__btn-close"),
            "is-open"
        ],
        [
            document.querySelectorAll(".material-open"),
            document.querySelector(".material-modal"),
            document.querySelector(".material__btn-close"),
            "is-open"
        ],
    ];

    // функция открытия модального окна
    const openModal = (modal, toggleClass) => {
        modal.classList.add(toggleClass);
    }
    // функция закрытия модального окна
    const closeModal = (modal, toggleClass) => {
        modal.classList.remove(toggleClass);
    }

    // функция для создания событий у элементов модального окна
    const changeStateModal = (arr, isHidden = false) => {
        // деструктуризация входного массива
        // ===========
        // 1. Кнопка/Кнопки открытия
        // 2. Модальное окно
        // 3. Кнопка скрытия
        // 4. Активный класс
        // 5. Не обязательный параметр
        // ===========
        const [btnOpen, modal, btnClose, activeClass] = arr;

        // открытие окна
        if(btnOpen) {
            btnOpen.forEach(btn=> {
                btn.addEventListener("click", ()=>openModal(modal, activeClass));
            });
        }
        // закрытие окна
        if(btnClose) {
            btnClose.addEventListener("click", ()=>closeModal(modal,activeClass));
        }
        
        // проверка параметра для выполнения функционала скрытия модального окна при клике вне него, и при нажатии кнопки ESC
        if(!isHidden) return;
        // скрытие при клике вне блока
        window.addEventListener("click", (event)=> {
            // проверка при клике в любое место на странице
            // если корневой эл. или тот по которому было нажатие соответсвует классу аккордеона
            // тогда оставлять класс, в противном случае класс удаляется
            if(!event.target.closest(`.${modal.classList[0]}`)) modal.classList.remove(activeClass);
        });
        // скрытие блока по нажатию на кнопку ESC
        window.addEventListener("keydown", (event)=> {
            if(event.code === "Escape" || event.keyCode === 27 || event.key === "Escape") {
                modal.classList.remove(activeClass);
            }
        });


    }

    // вызов функции для навешивания событий на элементы модального окна
    node.forEach(arr=> changeStateModal(arr));
}

export default modal;
</pre>

<h2>
 &#128241; Маска для телефона
</h2>

```
npm install imask
```
```
<script src="https://unpkg.com/imask"></script>
```

<pre>
    const phoneMask = () => {
    const nodes = document.querySelectorAll(".input-phone-mask");
      if(!nodes) return;
      
      const createMask = (el) => {
          const element = el;
          const maskOptions = {
              mask: '+{7}(000)000-00-00'
          };
          const mask = IMask(element, maskOptions);
      }
  
      nodes.forEach(node=> createMask(node));
    }

  export default phoneMask;
</pre>

<h2>
 &#8597; dropdown
</h2>

<pre>
  const accordions = () => {
    // ===================================

    // =========== функционал открытия аккордионов
    // в массив прокидывается:
    // 1. Список элемнетов
    // 2. Блок на который происходит нажатие
    // 3. Активный класс
    const nodesAccordion = [
        [
            document.querySelectorAll(".header__nav-accordion"), 
            ".header__nav-accordion-top", 
            "is-active"
        ],
        [
            document.querySelectorAll(".competencies__accordion"),
            ".competencies__accordion-top",
            "is-active"
        ]
    ];
    // arr - выше описанный массив
    // isHidden - параметр который будет включать/отключать функционал открытия/скрытия блока при клике вне него, и при нажатии на esc
    const accordisonsOpen = (arr, isHidden = false) => {

        arr[0].forEach(item=> {
            const top = item.querySelector(arr[1]);

            top.addEventListener("click", ()=> {
                arr[0].forEach(i=> {
                    if(item !== i) {
                        i.classList.remove(arr[2]);
                    }
                });

                item.classList.toggle(arr[2]);
            })
        });


        if(!isHidden) return;
        // скрытие на ESC
        window.addEventListener("keydown", (event)=> {
            if(event.code === "Escape" || event.keyCode === 27 || event.key === "Escape") {
                arr[0].forEach(item=> {
                    item.classList.remove(arr[2]);
                })
            }
        });
        
        // скрытие блока при клике вне блока
        window.addEventListener("click", (event)=> {
            arr[0].forEach(item=> {
                // проверка при клике в любое место на странице
                // если корневой эл. или тот по которому было нажатие соответсвует классу аккордеона
                // тогда оставлять класс, в противном случае класс удаляется
                if(!event.target.closest(`.${item.classList[0]}`)) item.classList.remove(arr[2]);
            });
        })
    }

    nodesAccordion.forEach(arr=> accordisonsOpen(arr, true));

    // ===================================

    // =========== функционал открытия аккордионов по id
    // в массив прокидывается:
    // 1. Список элемнетов (кнопок для окрытия), блок на который происходит нажатие
    // 2. Список блоков которые будут открываться когда id кнопки === id контента
    // 3. data атрибут у кнопок
    // 4. data атрибут у контента
    const nodesAccordionForId = [
        [
            document.querySelectorAll(".services__accordion-button"),
            document.querySelectorAll(".services__accordion-list"),
            "data-id",
            "data-body-id"
        ],
        [
            document.querySelectorAll(".experience__accordion-btn"),
            document.querySelectorAll(".experience__accordion-list"),
            "data-id",
            "data-body-id"
        ],
        [
            document.querySelectorAll(".service-first__top-btn"),
            document.querySelectorAll(".service-first__body"),
            "data-id",
            "data-body-id"
        ]
    ];

    const accordionsOpenData = (arr) => {
        const [buttons, bodys, dataBtn, dataBody] = arr;
        // функция при клике на верхние кнопки
        const handleClick = (btn, dataBtn, dataBody) => {
            btn.addEventListener("click", ()=> {
                buttons.forEach(b=>b.classList.remove("is-active"));
                bodys.forEach(b=>b.classList.remove("is-active"));

                const currentDataID = String(btn.getAttribute(dataBtn));

                bodys.forEach(body=> {
                    const bodyDataID = String(body.getAttribute(dataBody));

                    if(bodyDataID === currentDataID) body.classList.add("is-active");
                });

                btn.classList.add("is-active");
            })
        }

        // клик по кнопкам
        if(buttons) {
            buttons.forEach(btn=> handleClick(btn, dataBtn, dataBody));
        }
    }

    nodesAccordionForId.forEach(arr=> accordionsOpenData(arr));
}

export default accordions;
</pre>

<h2>
&#128287; Разделение числа на разряды
</h2>

<pre>
  function numberWithSpaces(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
  }
</pre>

<h2>
  	&#128722; Корзина
</h2>

<pre>
  // функция для работы с кнопками в элементе корзины
const changeStateItem = (item) => {
    // получение кнопок можно поменять на классы спокойно
    const btnP = item.querySelectorAll(".basket__item-counts-btn")[1],
        btnM = item.querySelectorAll(".basket__item-counts-btn")[0],
        countInput = item.querySelector(".basket__item-count")

    btnP.addEventListener("click", ()=>countChangeItem("+", item));
    btnM.addEventListener("click", ()=>countChangeItem("-", item));
    countInput.addEventListener("input", (e)=>countChangeItem("input", item));
}

const countChangeItem = (btn, item) => {
    let btnState = String(btn).trim().toLowerCase();
    let count = Number(item.getAttribute("data-count"));
    const price = Number(item.getAttribute("data-price"));
    const nodePrice = item.querySelector(".basket__item-price");
    const itemCountInput = item.querySelector(".basket__item-count");
    // получение кнопок можно поменять на классы спокойно
    const btnMinus = item.querySelectorAll(".basket__item-counts-btn")[0];
    const inputNode = item.querySelector(".basket__item-count");

    const plus = () =>{
        count+=1;

        const currentPrice = qsToString(count * price);

        item.setAttribute("data-count", qsToString(count));
        item.setAttribute("data-current-price", currentPrice);

        nodePrice.innerText = `${numberWithSpaces(currentPrice)} ₽`;

        itemCountInput.value = count;

        btnMinus.disabled = false;
        btnMinus.classList.add("is-active");
    }

    const minus = () => {
        count-=1;
        
        const currentPrice = qsToString(count * price);

        item.setAttribute("data-count", qsToString(count));
        item.setAttribute("data-current-price", currentPrice);

        nodePrice.innerText = `${numberWithSpaces(currentPrice)} ₽`;

        itemCountInput.value = count;

        if(count === 1) {
            btnMinus.disabled = true;
            btnMinus.classList.remove("is-active");
            return;
        }
    }

    const input = () => {
        let valueIn = inputNode.value;

        if(valueIn <= 1) {
            inputNode.value = 1;
            valueIn = 1;
            btnMinus.disabled = true;
            btnMinus.classList.remove("is-active");
        } else if(valueIn > 1) {
            btnMinus.disabled = false;
            btnMinus.classList.add("is-active");
        }
        
        const currentPrice = qsToString(valueIn * price);

        item.setAttribute("data-count", qsToString(valueIn));
        item.setAttribute("data-current-price", currentPrice);

        nodePrice.innerText = `${numberWithSpaces(currentPrice)} ₽`;
    }

    switch (btnState) {
        case "+":
            plus();
            break;
        case "-":
            minus();
            break;
        case "input":
            input();
            break;
        default:
            alert( "Нет таких значений" );
    }
}

basketItem.forEach(item=> changeStateItem(item));
</pre>

<h2>
  &#128065;&#65039;&#8205;&#128488;&#65039; Фокусировка на объекте
</h2>

<pre>
  input.onblur = function() {
  if (!input.value.includes('@')) { // не email
    input.classList.add('invalid');
    error.innerHTML = 'Пожалуйста, введите правильный email.'
  }
};

input.onfocus = function() {
  if (this.classList.contains('invalid')) {
    // удаляем индикатор ошибки, т.к. пользователь хочет ввести данные заново
    this.classList.remove('invalid');
    error.innerHTML = "";
  }
};
</pre>
<h2>
  &#128510; Замена изображений при перетаскивании ползунка
</h2>

<img src="https://raw.githubusercontent.com/lilkost/js-exx/refs/heads/main/image/img1.png">

<p>
  <a href="https://ru.stackoverflow.com/questions/1564462/%D0%9A%D0%B0%D0%BA-%D1%80%D0%B5%D0%B0%D0%BB%D0%B8%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D1%81%D0%BC%D0%B5%D0%BD%D1%83-%D0%BA%D0%B0%D1%80%D1%82%D0%B8%D0%BD%D0%BA%D0%B8-%D0%B2-%D0%B7%D0%B0%D0%B2%D0%B8%D1%81%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D0%B8-%D0%BE%D1%82-%D0%BF%D0%BE%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F-%D0%BF%D0%BE%D0%BB%D0%B7%D1%83%D0%BD%D0%BA%D0%B0" style="color: #007AFF;">Перейти по ссылке (ru.stackoverflow.com) </a>
</p>


<h2>
  &#8987;
  Получить текущее значение анимации
</h2>

<pre>
  // Функция для получения текущего значения свойства
    function getCurrentOpacity() {
        const currentOpacity = window.getComputedStyle(image).rotate;
        console.log('Текущая transform:', currentOpacity);
    }

    // Проверка значения каждые 100 мс во время анимации
    const intervalId = setInterval(() => {
        getCurrentOpacity();
    }, 1000);
</pre>

<h2>
  &#128083; Слежка за тем, появился ли блок в области видимости или нет
</h2>



```javascript
  // за каким блоком следить
  const targetBlock = document.querySelector('#targetBlock');
  let isInViewport = false;

  const callback = (entries) => {
      entries.forEach(entry => {
          if (entry.isIntersecting && !isInViewport) {
              console.log('Вы достигли блока!');
              isInViewport = true;
          } else if (!entry.isIntersecting && isInViewport) {
              console.log('Вы покинули блок!');
              isInViewport = false;
          }
      });
  };

  
  const observer = new IntersectionObserver(callback);
  
  if (targetBlock) {
      observer.observe(targetBlock);
  }
```


<h2>
  &#128083; Определение OS у пользователя
</h2>
