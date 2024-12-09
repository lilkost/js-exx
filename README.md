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
</code>
