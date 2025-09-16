<h2>
  &#128190; Копирование текста
</h2>

```javascript
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
```

<h2>
 &#127490; Модальные окна
</h2>

```scss
.modal{
	left: -200vw;
    &__inner {
        position: relative;

        transform: translateY(50px);
        opacity: 0;
        will-change: transform;

        transition: 
            transform $t-base, 
            opacity 200ms ease-in-out
        ;
    }
	    &.is-open {
        left: 0;
        .modal__inner{
            opacity: 1;
            transform: translateY(0px);
        }
    }
    &.is-back-animate{
        .modal__inner{
            opacity: 0;
            transform: translateY(50px);
        }
    }
}
```

```javascript
  const modal = () => {
    // Массив всех модалок на странице
    // 1. Кнопка/Кнопки открытия
    // 2. Модальное окно
    // 3. Кнопка скрытия
    // 4. Активный класс
    // 5. У родительского блока с модалкой должен быть класс - modal-parent
    // 6. Для обранй анимации класс - is-back-animate, для modal-parent
    // Кнопку/Кнопки открытия задавать через querySelectorAll
    const node = [
        [
            document.querySelectorAll('.btn-call-back'), 
            document.querySelector(".modal-call"), 
            document.querySelector(".modal-call .modal__btn-close"), 
            "is-open",
        ],
    ]

    // функция открытия модального окна
    const openModal = (modal, toggleClass) => {
        modal.classList.add(toggleClass);
    }
    // функция закрытия модального окна
    const closeModal = (modal, toggleClass) => {
        modal.classList.add("is-back-animate");

        setTimeout(()=>{
            modal.classList.remove(toggleClass);
            modal.classList.remove("is-back-animate");
        }, 400);
    }

    // функция для создания событий у элементов модального окна
    const changeStateModal = (arr, isHidden = true) => {
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
            if (event?.target?.classList?.value?.includes('modal-parent') && modal) closeModal(modal, activeClass);
        });
        // скрытие блока по нажатию на кнопку ESC
        window.addEventListener("keydown", (event)=> {
            if(event.code === "Escape" || event.keyCode === 27 || event.key === "Escape") {
                // modal.classList.remove(activeClass);
                closeModal(modal,activeClass)
            }
        });
    }
    // вызов функции для навешивания событий на элементы модального окна
    node.forEach(arr=> changeStateModal(arr));
}

modal();
```

<h2>
 &#128241; Маска для телефона
</h2>

```
npm install imask
```
```
<script src="https://unpkg.com/imask"></script>
```

```javascript
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
```

<h2>
 &#8597; dropdown
</h2>

```javascript
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
```

<h2>
&#128287; Разделение числа на разряды
</h2>

```javascript
  function numberWithSpaces(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
  }
```

<h2>
  	&#128722; Корзина
</h2>

```javascript
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
```

<h2>
  &#128065;&#65039;&#8205;&#128488;&#65039; Фокусировка на объекте
</h2>

```javascript
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
```

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

```javascript
  // Функция для получения текущего значения свойства
    function getCurrentOpacity() {
        const currentOpacity = window.getComputedStyle(image).rotate;
        console.log('Текущая transform:', currentOpacity);
    }

    // Проверка значения каждые 100 мс во время анимации
    const intervalId = setInterval(() => {
        getCurrentOpacity();
    }, 1000);
```

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
  	&#127760; Определение OS у пользователя
</h2>

```javascript
  function setOSClassOnBody() {
      const userAgent = window.navigator.userAgent;
      let osClass = "";
  
      if (userAgent.indexOf("Win") !== -1) {
          osClass = "os-windows";
      } else if (userAgent.indexOf("Mac") !== -1) {
          osClass = "os-macos";
      } else if (userAgent.indexOf("X11") !== -1 || userAgent.indexOf("Linux") !== -1) {
          osClass = "os-linux";
      } else if (userAgent.indexOf("Android") !== -1) {
          osClass = "os-android";
      } else if (userAgent.indexOf("like Mac") !== -1) {
          osClass = "os-ios";
      } else {
          osClass = "os-unknown"; // На случай, если ОС не распознана
      }
  
      // Добавляем класс к тегу body
      document.body.classList.add(osClass);
  }
  
  // Вызываем функцию при загрузке страницы
  document.addEventListener("DOMContentLoaded",()=>setOSClassOnBody());
  }
```

<h2>
  &#128260; Конструктор slider'ов
</h2>

```javascript
    // массив всех слайдеров
    // для создания простых слайдеров без сложной логики
    const sliders = [
        // пример слайдера
           [
            document.querySelector(".what-doing__slider"),
            {
                direction: 'horizontal',
                loop: true,
                navigation: {
                    nextEl: '.what-doing__slider-btn_next',
                    prevEl: '.what-doing__slider-btn_prev',
                },
                slidesPerView: 1.81559,
            }
        ],
    ]
    // функция конструктор для создания сладеров
    const createSlider = (node, options) => {
        if(node && options) {
            const swiper = new Swiper(node, options);
        }
        else {
            console.error("Ошибка генерации слайдера");
        }
    }
    // вызов конструктора для слайдеров
    if(sliders && sliders.length) {
        sliders.forEach(slider=> {
            const sliderNode = slider[0];
            const sliderOptions = slider[1];

            if(sliderNode && sliderOptions) {
                createSlider(sliderNode, sliderOptions);
            }
            else {
                console.error(`Ошибка генерации, нету одной из двух частей слайдера: slider - ${sliderNode}, список опций - ${sliderOptions}`)
            }
        });
    }
```

<h2>
  &#128506; Запрет действия с картами
</h2>

```css
.parent-map:not(.is-active) * {
    pointer-events: none;
}
```

```javascript
const scrollMap = () =>{
    // обертка карты
    const wrapperMap = document.querySelector(".parent-map");

    if(!wrapperMap) return; 

    // действия на странице
    document.addEventListener("click", (event) => {
        wrapperMap.classList.toggle("is-active", event.target === wrapperMap);
    });
}

export default scrollMap;
```

<h2>
  &#127850; Модальное окно с cookie
</h2>


```PHP
<? if($_COOKIE['is_show_modal_coockie'] !== 'Y') {?>
    <div class="coockie-modal">
        <p class="coockie-modal__text">
            Мы используем файлы cookie для улучшения работы сайта, пользуясь сайтом вы соглашаетесь с <a href="https://malt.ru/usloviya-obrabotki-personalnykh-dannykh/">политикой использования cookie</a>
        </p>
        <button class="coockie-modal__btn" type="button" id="coockieModalBtn">Хорошо</button>
    </div>
<?}?>
```

```css
.coockie-modal {
    max-width: 300px;
    border-radius: 20px;
    background-color: #000;
    padding: 25px;
    position: fixed;
    z-index: 100;
    bottom: 3%;
    animation: 2.5s linear 0s forwards alternate leftPos;
}
.coockie-modal__text {
    color: #fff;
    font-size: 16px;
    font-weight: 400;
    margin-bottom: 30px;
}
.coockie-modal__text a {
    color: currentColor;
    transition: color 300ms linear;
}

.coockie-modal__btn {
    max-width: 100%;
    width: 100%;
    font-size: 16px;
    line-height: 40px;
    font-weight: 400;
    color: white;
    text-transform: uppercase;
    display: flex
;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background: linear-gradient(#75a526, #8bbc3e);
    padding: 0 20px;
    border-radius: 25px;
    outline: none;
    border: none;
    cursor: pointer;
    transition: opacity 300ms linear;
    opacity: 1;
}
.coockie-modal.is-hidden {
    display: none !important;
}

@keyframes leftPos {
	0% {
		left: -100%;
		opacity: 0;
	}
	100% {
		left: 3%;
		opacity: 1;
	}
}
```
```JS
const modal = document.querySelector(".coockie-modal");

if(modal){
    const modalCLose = modal.querySelector(".coockie-modal__btn");

    const setItemCoockie = () => {
        let date = new Date(Date.now() + 31536000000);
        date = date.toUTCString();

        document.cookie = "is_show_modal_coockie=Y;max-age=31536000000";

        modal.classList.add("is-hidden");
    }

    modalCLose.addEventListener("click", ()=>setItemCoockie());
}
```

<h2>
  &#128357; Получение даты
</h2>

```JS
new Intl.DateTimeFormat('en-US', {
        year: "numeric",
        day: "numeric",
        month: "short",
    }).format(date);
```

<h2>
  	&#127754; Бегущая строка на ваниле
</h2>

```CSS
 .marquee {
      /* Скорость: чем больше время — тем медленнее прокрутка */
      --marquee-duration: 20s;

      /* Направление: 1 — слева направо? (на деле это «влево»),
         -1 — наоборот (вправо). Подробнее ниже. */
      --marquee-direction: 1;

      /* Отступы между элементами внутри группы */
      --marquee-gap: 2rem;

      position: relative;
      overflow: hidden;
      display: block;
      line-height: 1;
      background: #0b0c10;
      color: #fff;
      font: 16px/1.2 system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      padding: 10px 0;

      -webkit-mask-image: linear-gradient(90deg, transparent, #000 6%, #000 94%, transparent);
              mask-image: linear-gradient(90deg, transparent, #000 6%, #000 94%, transparent);
    }

    .marquee__track {
      display: flex;
      width: max-content;
      will-change: transform;  
      animation: marquee var(--marquee-duration) linear infinite;
    }


    .marquee:hover .marquee__track,
    .marquee:focus-within .marquee__track {
      animation-play-state: paused;
    }

    .marquee__group {
      display: flex;
      align-items: center;
    }

    .marquee__item {
      white-space: nowrap;
      margin-right: var(--marquee-gap);
      display: inline-flex;
      align-items: center;
      gap: .5rem;
    }
    .marquee__item:last-child { margin-right: 0; }


    @keyframes marquee {
      from { transform: translateX(0); }
      to   { transform: translateX(calc(-50% * var(--marquee-direction))); }
    }

    @media (prefers-reduced-motion: reduce) {
      .marquee__track { animation: none; }
    }
```

```HTML
<div class="marquee" style="--marquee-duration: 18s; --marquee-direction: 1;" aria-label="Горячие скидки до 50 процентов. Бесплатная доставка от трёх тысяч рублей. Новые поступления каждую неделю.">
    <div class="marquee__track">

      <!-- Группа №1 -->
      <div class="marquee__group">
        <div class="marquee__item">123</div>
		<div class="marquee__item">123</div>
		<div class="marquee__item">123</div>
      </div>

      <!-- Группа №2 (строго идентична первой!) -->
      <div class="marquee__group" aria-hidden="true">
        <div class="marquee__item">123</div>
		<div class="marquee__item">123</div>
		<div class="marquee__item">123</div>
      </div>
    </div>
  </div>
```
<h2>
  	&#128104;&#8205;&#128187; Слайдер без использования библиотек
</h2>

```HTML
	<div class="slider">
	    <div class="slider-track">
	      <div class="slide">1</div>
	      <div class="slide">2</div>
	      <div class="slide">3</div>
	      <div class="slide">4</div>
	    </div>
	    <button class="slider-btn prev">‹</button>
	    <button class="slider-btn next">›</button>
	  </div>
	  <div class="pagination"></div>
```

```CSS
    .slider {
      position: relative;
      width: 80%;
      overflow: hidden;
    }

    .slider-track {
      display: flex;
      transition: transform 0.5s ease;
    }

    .slide {
      min-width: calc(100% / 3);
      flex-shrink: 0;
      background: #ddd;
      border-radius: 12px;
      margin: 0 10px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 24px;
      font-weight: bold;
      height: 200px;
    }

    /* 2 слайда на средних */
    @media (max-width: 992px) {
      .slide {
        min-width: calc(100% / 2);
      }
    }

    /* 1 слайд на мобильных */
    @media (max-width: 600px) {
      .slide {
        min-width: 100%;
      }
    }

    .slider-btn {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(0,0,0,0.5);
      border: none;
      color: #fff;
      font-size: 20px;
      padding: 10px 15px;
      cursor: pointer;
      border-radius: 50%;
    }

    .prev {
      left: 10px;
    }

    .next {
      right: 10px;
    }

    .pagination {
      text-align: center;
      margin-top: 10px;
      font-size: 18px;
    }
```
<p>Стандартный слайдер</p>

```JS
  const track = document.querySelector('.slider-track');
    const slides = document.querySelectorAll('.slide');
    const prevBtn = document.querySelector('.prev');
    const nextBtn = document.querySelector('.next');
    const pagination = document.querySelector('.pagination');

    let index = 0;
    let slidesToShow = getSlidesToShow();
    const totalSlides = slides.length;
    let autoPlayInterval;

    function getSlidesToShow() {
      if (window.innerWidth <= 600) return 1;
      if (window.innerWidth <= 992) return 2;
      return 3;
    }

    function updateSlider() {
      const slideWidth = slides[0].offsetWidth + 20; // ширина + margin
      track.style.transform = `translateX(-${index * slideWidth}px)`;
      pagination.textContent = `${Math.floor(index / slidesToShow) + 1} / ${Math.ceil(totalSlides / slidesToShow)}`;
    }

    function nextSlide() {
      index += slidesToShow;
      if (index >= totalSlides) index = 0; // зацикливание
      updateSlider();
    }

    function prevSlide() {
      index -= slidesToShow;
      if (index < 0) {
        // идём в конец
        index = (Math.ceil(totalSlides / slidesToShow) - 1) * slidesToShow;
      }
      updateSlider();
    }

    function startAutoplay() {
      autoPlayInterval = setInterval(nextSlide, 4000);
    }

    function stopAutoplay() {
      clearInterval(autoPlayInterval);
    }

    nextBtn.addEventListener('click', () => {
      stopAutoplay();
      nextSlide();
      startAutoplay();
    });

    prevBtn.addEventListener('click', () => {
      stopAutoplay();
      prevSlide();
      startAutoplay();
    });

    window.addEventListener('resize', () => {
      slidesToShow = getSlidesToShow();
      index = 0; // сбрасываем позицию при ресайзе
      updateSlider();
    });

    // init
    updateSlider();
    startAutoplay();
```

<p>Слайдер для мобилки</p>

```CSS
 .slider {
      position: relative;
      overflow: hidden;
      width: 100vw;
    }

    .slider-track {
      display: flex;
      transition: transform 0.5s ease;
      gap: 10px; /* отступы между слайдами */
    }

    .slide {
      flex: 0 0 100vw; /* ширина = 100vw */
      height: 200px;
      border-radius: 12px;
      background: #ddd;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 28px;
      font-weight: bold;
    }

    .slider-btn {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(0,0,0,0.5);
      border: none;
      color: #fff;
      font-size: 20px;
      padding: 10px 15px;
      cursor: pointer;
      border-radius: 50%;
      z-index: 2;
    }

    .prev { left: 10px; }
    .next { right: 10px; }

    .pagination {
      text-align: center;
      margin: 15px 0;
    }

    .bullet {
      display: inline-block;
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background: #bbb;
      margin: 0 5px;
      cursor: pointer;
      transition: background 0.3s, transform 0.3s;
    }

    .bullet.active {
      background: #333;
      transform: scale(1.2);
    }

    /* На экранах >= 480px показываем все слайды */
    @media (min-width: 480px) {
      .slider-track {
        transform: none !important;
        transition: none !important;
        flex-wrap: wrap;
      }
      .slide {
        flex: 1 1 calc(33% - 10px); /* например сетка 3 в ряд */
      }
      .slider-btn, .pagination {
        display: none !important;
      }
    }
```
```JS
const track = document.querySelector('.slider-track');
const slides = document.querySelectorAll('.slide');
const prevBtn = document.querySelector('.prev');
const nextBtn = document.querySelector('.next');
const pagination = document.querySelector('.pagination');

let index = 0;
const totalSlides = slides.length;
let bullets = [];

function initSlider() {
  if (window.innerWidth < 480) {
    pagination.innerHTML = "";
    for (let i = 0; i < totalSlides; i++) {
      const bullet = document.createElement('span');
      bullet.classList.add('bullet');
      if (i === 0) bullet.classList.add('active');
      bullet.addEventListener('click', () => goToSlide(i));
      pagination.appendChild(bullet);
    }
    bullets = document.querySelectorAll('.bullet');

    prevBtn.style.display = "block";
    nextBtn.style.display = "block";
    updateSlider();

    enableSwipe(); // подключаем свайпы
  } else {
    track.style.transform = "none";
    pagination.innerHTML = "";
    prevBtn.style.display = "none";
    nextBtn.style.display = "none";
  }
}

function updateSlider() {
  if (window.innerWidth >= 480) return;
  const slideWidth = slides[0].offsetWidth + 10; // учитываем gap
  track.style.transform = `translateX(-${index * slideWidth}px)`;

  bullets.forEach(b => b.classList.remove('active'));
  if (bullets[index]) bullets[index].classList.add('active');

  prevBtn.disabled = index === 0;
  nextBtn.disabled = index === totalSlides - 1;
}

function nextSlide() {
  if (index < totalSlides - 1) {
    index++;
    updateSlider();
  }
}

function prevSlide() {
  if (index > 0) {
    index--;
    updateSlider();
  }
}

function goToSlide(i) {
  index = i;
  updateSlider();
}

nextBtn.addEventListener('click', nextSlide);
prevBtn.addEventListener('click', prevSlide);

window.addEventListener('resize', () => {
  index = 0;
  initSlider();
});

// ========== свайпы ==========
function enableSwipe() {
  let startX = 0;
  let endX = 0;

  track.addEventListener('touchstart', (e) => {
    startX = e.touches[0].clientX;
  });

  track.addEventListener('touchend', (e) => {
    endX = e.changedTouches[0].clientX;
    const deltaX = endX - startX;

    if (Math.abs(deltaX) > 50) { // порог, чтобы избежать ложных срабатываний
      if (deltaX < 0) {
        nextSlide(); // свайп влево → следующий слайд
      } else {
        prevSlide(); // свайп вправо → предыдущий слайд
      }
    }
  });
}

// init
initSlider();
```
