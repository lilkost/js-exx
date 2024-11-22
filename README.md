<h2>
  &#128190; Конпирование текста
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
