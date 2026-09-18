/* =========================================================
   UNILUB - SISTEMA DE ESTOQUE
   VERSÃO LOCAL / DEMONSTRAÇÃO
========================================================= */


/* =========================================================
   USUÁRIAS
========================================================= */

const USERS = {

    Laura: "Laura123!",

    Sarah: "Sarah456!",

    Ana: "Ana789!",

    Luara: "Luara321!",

    Elisangela: "Elisangela654!",

    Viviani: "Viviani987!",

    Junior: "Junior159!"

};


/* =========================================================
   BANCO LOCAL
========================================================= */

const STORAGE_KEY = "unilub_estoque_v2";


let db = JSON.parse(
    localStorage.getItem(STORAGE_KEY)
    ||
    JSON.stringify({

        products: [],

        materials: [],

        clients: [],

        movements: [],

        sales: [],

        dailyNotes: [],

        occurrences: []

    })
);


/* Compatibilidade com bancos criados em versões anteriores */

db.products =
    Array.isArray(db.products)
        ? db.products
        : [];

db.materials =
    Array.isArray(db.materials)
        ? db.materials
        : [];

db.clients =
    Array.isArray(db.clients)
        ? db.clients
        : [];

db.movements =
    Array.isArray(db.movements)
        ? db.movements
        : [];

db.sales =
    Array.isArray(db.sales)
        ? db.sales
        : [];

db.dailyNotes =
    Array.isArray(db.dailyNotes)
        ? db.dailyNotes
        : [];

db.occurrences =
    Array.isArray(db.occurrences)
        ? db.occurrences
        : [];


let currentUser =
    sessionStorage.getItem(
        "unilub_user"
    ) || "";


/* =========================================================
   PERMISSÕES DE EDIÇÃO DE PRODUTOS
========================================================= */

const PRODUCT_EDITORS = [
    "Laura",
    "Ana",
    "Sarah",
    "Luara"
];


function canEditProducts() {

    return PRODUCT_EDITORS.includes(
        currentUser
    );

}


/* =========================================================
   FUNÇÕES BÁSICAS
========================================================= */

function $(id) {

    return document.getElementById(id);

}


function saveDatabase() {

    localStorage.setItem(
        STORAGE_KEY,
        JSON.stringify(db)
    );

}


function generateId() {

    return (
        Date.now().toString(36)
        +
        Math.random()
            .toString(36)
            .substring(2)
    );

}


function today() {

    return new Date()
        .toISOString()
        .slice(0, 10);

}


function formatNumber(value) {

    return Number(value || 0)
        .toLocaleString(
            "pt-BR",
            {
                maximumFractionDigits: 2
            }
        );

}


function escapeHTML(value) {

    return String(value ?? "")
        .replace(
            /[&<>"']/g,

            function(character) {

                return {

                    "&": "&amp;",

                    "<": "&lt;",

                    ">": "&gt;",

                    '"': "&quot;",

                    "'": "&#039;"

                }[character];

            }
        );

}


/* =========================================================
   STATUS DO ESTOQUE
========================================================= */

function getStatus(
    stock,
    minimum
) {

    if (stock <= 0) {

        return {

            text: "Sem estoque",

            className: "status-out"

        };

    }


    if (stock <= minimum) {

        return {

            text: "Estoque baixo",

            className: "status-low"

        };

    }


    return {

        text: "Normal",

        className: "status-ok"

    };

}


/* =========================================================
   LEITURA DA IMAGEM
========================================================= */

function readImage(file) {

    return new Promise(

        function(resolve, reject) {

            const reader =
                new FileReader();


            reader.onload =
                function() {

                    resolve(
                        reader.result
                    );

                };


            reader.onerror =
                reject;


            reader.readAsDataURL(
                file
            );

        }

    );

}


/* =========================================================
   LOGIN
========================================================= */

function setupLogin() {

    if (!$("loginForm")) {
        return;
    }


    $("loginForm")
        .addEventListener(
            "submit",
            function(event) {

                event.preventDefault();


                const username =
                    $("loginUser").value;


                const password =
                    $("loginPassword").value;


                if (
                    USERS[username]
                    &&
                    USERS[username] === password
                ) {

                    currentUser =
                        username;


                    sessionStorage.setItem(
                        "unilub_user",
                        username
                    );


                    $("loginScreen")
                        .classList
                        .add("hidden");


                    $("app")
                        .classList
                        .remove("hidden");


                    initializeApp();

                }

                else {

                    $("loginError")
                        .textContent =
                        "Usuária ou senha incorreta.";

                }

            }
        );

}


/* =========================================================
   INICIALIZAÇÃO
========================================================= */

function initializeApp() {

    if ($("currentUser")) {

        $("currentUser")
            .textContent =
            currentUser;

    }


    if ($("headerUser")) {

        $("headerUser")
            .textContent =
            currentUser;

    }


    /* Somente Laura, Ana, Sarah e Luara
       podem alterar produtos. */

    if ($("newProductBtn")) {

        $("newProductBtn")
            .style
            .display =
            canEditProducts()
                ? ""
                : "none";

    }


    if ($("entryDate")) {

        $("entryDate")
            .value =
            today();

    }


    if ($("exitDate")) {

        $("exitDate")
            .value =
            today();

    }


    if ($("saleDate")) {

        $("saleDate")
            .value =
            today();

    }


    if ($("dailyNoteDate")) {

        $("dailyNoteDate")
            .value =
            today();

    }


    if ($("occurrenceDate")) {

        $("occurrenceDate")
            .value =
            today();

    }


    setupNavigation();

    setupForms();

    setupSalesForms();

    setupOccurrenceForms();

    setupSearches();

    renderAll();

}


/* =========================================================
   NAVEGAÇÃO
========================================================= */

function setupNavigation() {

    document
        .querySelectorAll(
            ".nav-item"
        )
        .forEach(

            function(button) {

                button.onclick =
                    function() {

                        showPage(
                            button.dataset.page
                        );

                    };

            }

        );


    document
        .querySelectorAll(
            "[data-go]"
        )
        .forEach(

            function(button) {

                button.onclick =
                    function() {

                        showPage(
                            button.dataset.go
                        );

                    };

            }

        );

}


function showPage(pageId) {

    document
        .querySelectorAll(
            ".page"
        )
        .forEach(

            function(page) {

                page.classList
                    .remove(
                        "active-page"
                    );

            }

        );


    const selectedPage =
        $(pageId);


    if (selectedPage) {

        selectedPage.classList
            .add(
                "active-page"
            );

    }


    document
        .querySelectorAll(
            ".nav-item"
        )
        .forEach(

            function(button) {

                button.classList.toggle(

                    "active",

                    button.dataset.page
                    ===
                    pageId

                );

            }

        );


    const titles = {

        dashboard: [
            "Dashboard",
            "Visão geral do estoque"
        ],

        products: [
            "Estoque de Produtos",
            "Quantidade atual disponível"
        ],

        materials: [
            "Materiais de Expedição",
            "Caixinhas, fitas, etiquetas e embalagens"
        ],

        entries: [
            "Entradas",
            "Registre recebimentos"
        ],

        exits: [
            "Saídas",
            "Registre pedidos enviados aos clientes"
        ],

        clients: [
            "Clientes",
            "Cadastro de clientes"
        ],

        history: [
            "Histórico",
            "Movimentações registradas"
        ],

        sales: [
            "Notas / Vendas",
            "Notas expedidas e histórico de vendas"
        ],

        occurrences: [
            "Ocorrências",
            "Pedidos errados, atendimentos e Melhor Envio"
        ]

    };


    if (
        titles[pageId]
        &&
        $("pageTitle")
        &&
        $("pageSubtitle")
    ) {

        $("pageTitle")
            .textContent =
            titles[pageId][0];


        $("pageSubtitle")
            .textContent =
            titles[pageId][1];

    }


    renderAll();

}


/* =========================================================
   RENDERIZAÇÃO GERAL
========================================================= */

function renderAll() {

    renderDashboard();

    renderProducts();

    renderMaterials();

    renderSelects();

    renderClients();

    renderHistory();

    renderSales();

    renderDailyNotes();

    renderOccurrences();

    renderSaleBoxSelect();

}


/* =========================================================
   DASHBOARD
========================================================= */

function renderDashboard() {

    const lowProducts =
        db.products.filter(

            function(product) {

                return (
                    Number(product.stock || 0) > 0
                    &&
                    Number(product.stock || 0)
                    <=
                    Number(product.min || 0)
                );

            }

        ).length;


    const outProducts =
        db.products.filter(

            function(product) {

                return Number(
                    product.stock || 0
                ) <= 0;

            }

        ).length;


    if ($("dashProducts")) {

        $("dashProducts")
            .textContent =
            db.products.length;

    }


    if ($("dashLow")) {

        $("dashLow")
            .textContent =
            lowProducts;

    }


    if ($("dashOut")) {

        $("dashOut")
            .textContent =
            outProducts;

    }


    if ($("dashMaterials")) {

        $("dashMaterials")
            .textContent =
            db.materials.length;

    }


    /* =====================================================
       ESTOQUE DO DASHBOARD
    ===================================================== */

    if ($("dashboardStock")) {

        if (!db.products.length) {

            $("dashboardStock")
                .innerHTML =
                `
                    <div class="empty">
                        Nenhum produto cadastrado.
                    </div>
                `;

        }

        else {

            const products =
                db.products.slice(
                    0,
                    8
                );


            $("dashboardStock")
                .innerHTML =

                `
                    <div class="mini-list">

                        ${
                            products
                                .map(

                                    function(product) {

                                        return `

                                            <div class="mini-item">

                                                <span>
                                                    ${escapeHTML(
                                                        product.name
                                                    )}
                                                </span>

                                                <strong>
                                                    ${formatNumber(
                                                        product.stock
                                                    )}
                                                    ${escapeHTML(
                                                        product.unit
                                                        ||
                                                        "un."
                                                    )}
                                                </strong>

                                            </div>

                                        `;

                                    }

                                )
                                .join("")
                        }

                    </div>
                `;

        }

    }


    /* =====================================================
       MATERIAIS DO DASHBOARD
    ===================================================== */

    if ($("dashboardMaterials")) {

        if (!db.materials.length) {

            $("dashboardMaterials")
                .innerHTML =
                `
                    <div class="empty">
                        Nenhum material cadastrado.
                    </div>
                `;

        }

        else {

            $("dashboardMaterials")
                .innerHTML =

                `
                    <div class="mini-list">

                        ${
                            db.materials
                                .slice(
                                    0,
                                    8
                                )
                                .map(

                                    function(material) {

                                        return `

                                            <div class="mini-item">

                                                <span>
                                                    ${escapeHTML(
                                                        material.name
                                                    )}
                                                </span>

                                                <strong>
                                                    ${formatNumber(
                                                        material.stock
                                                    )}
                                                    ${escapeHTML(
                                                        material.unit
                                                        ||
                                                        "un."
                                                    )}
                                                </strong>

                                            </div>

                                        `;

                                    }

                                )
                                .join("")
                        }

                    </div>
                `;

        }

    }

}


/* =========================================================
   PRODUTOS
========================================================= */

function renderProducts() {

    const table =
        $("productsTable");


    if (!table) {
        return;
    }


    const search =
        (
            $("productSearch")?.value
            ||
            ""
        )
        .toLowerCase()
        .trim();


    const products =
        db.products.filter(

            function(product) {

                const text =
                    (
                        product.name
                        +
                        " "
                        +
                        product.code
                        +
                        " "
                        +
                        (
                            product.category
                            ||
                            ""
                        )
                        +
                        " "
                        +
                        (
                            product.brand
                            ||
                            ""
                        )
                    )
                    .toLowerCase();


                return text.includes(
                    search
                );

            }

        );


    if (!products.length) {

        table.innerHTML = `

            <tr>

                <td
                    colspan="8"
                    class="empty"
                >
                    Nenhum produto encontrado.
                </td>

            </tr>

        `;

        return;

    }


    table.innerHTML =

        products.map(

            function(product) {

                const status =
                    getStatus(
                        Number(
                            product.stock
                            ||
                            0
                        ),
                        Number(
                            product.min
                            ||
                            0
                        )
                    );


                let imageHTML =
                    "—";


                if (product.image) {

                    imageHTML = `

                        <img
                            src="${escapeHTML(
                                product.image
                            )}"
                            class="product-image"
                            alt="Produto"
                        >

                    `;

                }


                return `

                    <tr>

                        <td>
                            ${imageHTML}
                        </td>

                        <td>

                            <strong>
                                ${escapeHTML(
                                    product.name
                                )}
                            </strong>

                            <br>

                            <small>
                                ${escapeHTML(
                                    product.category
                                    ||
                                    ""
                                )}
                            </small>

                        </td>

                        <td>
                            ${escapeHTML(
                                product.code
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                product.unit
                                ||
                                "un."
                            )}
                        </td>

                        <td>

                            <strong>
                                ${formatNumber(
                                    product.stock
                                )}
                            </strong>

                        </td>

                        <td>
                            ${formatNumber(
                                product.min
                            )}
                        </td>

                        <td>

                            <span
                                class="status ${status.className}"
                            >
                                ${status.text}
                            </span>

                        </td>

                        <td>

                            ${
                                canEditProducts()

                                ?

                                `

                                    <button
                                        class="btn btn-primary btn-sm"
                                        onclick="editProduct('${product.id}')"
                                    >
                                        Editar
                                    </button>

                                    <button
                                        class="delete-btn"
                                        onclick="deleteProduct('${product.id}')"
                                    >
                                        Excluir
                                    </button>

                                `

                                :

                                `

                                    <span
                                        class="readonly-badge"
                                    >
                                        Somente leitura
                                    </span>

                                `
                            }

                        </td>

                    </tr>

                `;

            }

        ).join("");

}


/* =========================================================
   EDITAR PRODUTO
========================================================= */

function editProduct(id) {

    if (!canEditProducts()) {

        alert(
            "Seu acesso é somente para leitura dos produtos."
        );

        return;

    }


    const product =
        db.products.find(

            function(item) {

                return item.id === id;

            }

        );


    if (!product) {

        alert(
            "Produto não encontrado."
        );

        return;

    }


    openModal(

        "Editar produto",

        `

            <div class="form-grid">

                <div class="form-group">

                    <label>
                        Nome do produto
                    </label>

                    <input
                        name="name"
                        value="${escapeHTML(
                            product.name
                        )}"
                        required
                    >

                </div>


                <div class="form-group">

                    <label>
                        Código
                    </label>

                    <input
                        name="code"
                        value="${escapeHTML(
                            product.code
                        )}"
                        required
                    >

                </div>


                <div class="form-group">

                    <label>
                        Categoria
                    </label>

                    <input
                        name="category"
                        value="${escapeHTML(
                            product.category
                            ||
                            ""
                        )}"
                    >

                </div>


                <div class="form-group">

                    <label>
                        Marca / Fabricante
                    </label>

                    <input
                        name="brand"
                        value="${escapeHTML(
                            product.brand
                            ||
                            ""
                        )}"
                    >

                </div>


                <div class="form-group">

                    <label>
                        Unidade
                    </label>

                    <select name="unit">

                        <option
                            value="un."
                            ${
                                product.unit === "un."
                                ? "selected"
                                : ""
                            }
                        >
                            Unidade
                        </option>

                        <option
                            value="kg"
                            ${
                                product.unit === "kg"
                                ? "selected"
                                : ""
                            }
                        >
                            kg
                        </option>

                        <option
                            value="L"
                            ${
                                product.unit === "L"
                                ? "selected"
                                : ""
                            }
                        >
                            Litro
                        </option>

                        <option
                            value="balde"
                            ${
                                product.unit === "balde"
                                ? "selected"
                                : ""
                            }
                        >
                            Balde
                        </option>

                        <option
                            value="tambor"
                            ${
                                product.unit === "tambor"
                                ? "selected"
                                : ""
                            }
                        >
                            Tambor
                        </option>

                        <option
                            value="caixa"
                            ${
                                product.unit === "caixa"
                                ? "selected"
                                : ""
                            }
                        >
                            Caixa
                        </option>

                    </select>

                </div>


                <div class="form-group">

                    <label>
                        Estoque
                    </label>

                    <input
                        name="stock"
                        type="number"
                        min="0"
                        step="0.01"
                        value="${Number(
                            product.stock
                            ||
                            0
                        )}"
                    >

                </div>


                <div class="form-group">

                    <label>
                        Estoque mínimo
                    </label>

                    <input
                        name="min"
                        type="number"
                        min="0"
                        step="0.01"
                        value="${Number(
                            product.min
                            ||
                            0
                        )}"
                    >

                </div>


                <div class="form-group">

                    <label>
                        Localização
                    </label>

                    <input
                        name="location"
                        value="${escapeHTML(
                            product.location
                            ||
                            ""
                        )}"
                    >

                </div>


                <div class="form-group">

                    <label>
                        Lote
                    </label>

                    <input
                        name="lot"
                        value="${escapeHTML(
                            product.lot
                            ||
                            ""
                        )}"
                    >

                </div>


                <div class="form-group">

                    <label>
                        Validade
                    </label>

                    <input
                        name="expiry"
                        type="date"
                        value="${escapeHTML(
                            product.expiry
                            ||
                            ""
                        )}"
                    >

                </div>


                <div class="form-group form-full">

                    <label>
                        Informações técnicas
                    </label>

                    <textarea
                        name="technical"
                    >${escapeHTML(
                        product.technical
                        ||
                        ""
                    )}</textarea>

                </div>


                <div class="form-group form-full">

                    <label>
                        Observações
                    </label>

                    <textarea
                        name="notes"
                    >${escapeHTML(
                        product.notes
                        ||
                        ""
                    )}</textarea>

                </div>


                <div class="form-group form-full">

                    <label>
                        Nova imagem
                    </label>

                    <input
                        type="file"
                        name="image"
                        accept="image/*"
                    >

                </div>


                <div class="form-actions form-full">

                    <button
                        class="btn btn-primary"
                        type="submit"
                    >
                        Salvar alterações
                    </button>

                </div>

            </div>

        `,

        async function(formData) {

            const imageFile =
                formData.get(
                    "image"
                );


            let image =
                product.image
                ||
                "";


            if (
                imageFile
                &&
                imageFile.size
            ) {

                image =
                    await readImage(
                        imageFile
                    );

            }


            product.name =
                formData.get(
                    "name"
                );


            product.code =
                formData.get(
                    "code"
                );


            product.category =
                formData.get(
                    "category"
                );


            product.brand =
                formData.get(
                    "brand"
                );


            product.unit =
                formData.get(
                    "unit"
                );


            product.stock =
                Number(
                    formData.get(
                        "stock"
                    )
                );


            product.min =
                Number(
                    formData.get(
                        "min"
                    )
                );


            product.location =
                formData.get(
                    "location"
                );


            product.lot =
                formData.get(
                    "lot"
                );


            product.expiry =
                formData.get(
                    "expiry"
                );


            product.technical =
                formData.get(
                    "technical"
                );


            product.notes =
                formData.get(
                    "notes"
                );


            product.image =
                image;


            saveDatabase();

        }

    );

}


/* =========================================================
   MODAL
========================================================= */

function openModal(
    title,
    formHTML,
    callback
) {

    if (!$("modal")) {
        return;
    }


    $("modalTitle")
        .textContent =
        title;


    $("modalForm")
        .innerHTML =
        formHTML;


    $("modal")
        .classList
        .remove("hidden");


    $("modalForm")
        .onsubmit =
        async function(event) {

            event.preventDefault();


            const formData =
                new FormData(
                    event.target
                );


            await callback(
                formData
            );


            $("modal")
                .classList
                .add("hidden");


            renderAll();

        };

}


/* =========================================================
   FECHAR MODAL
========================================================= */

if ($("modalClose")) {

    $("modalClose")
        .onclick =
        function() {

            $("modal")
                .classList
                .add("hidden");

        };

}


if ($("modal")) {

    $("modal")
        .addEventListener(
            "click",
            function(event) {

                if (
                    event.target
                    ===
                    $("modal")
                ) {

                    $("modal")
                        .classList
                        .add("hidden");

                }

            }
        );

}


/* =========================================================
   NOVO PRODUTO
========================================================= */

if ($("newProductBtn")) {

    $("newProductBtn")
        .onclick =
        function() {

            if (!canEditProducts()) {

                alert(
                    "Seu acesso é somente para leitura dos produtos."
                );

                return;

            }


            openModal(

                "Cadastrar novo produto",

                `

                    <div class="form-grid">

                        <div class="form-group">

                            <label>
                                Nome do produto
                            </label>

                            <input
                                name="name"
                                required
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Código
                            </label>

                            <input
                                name="code"
                                required
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Categoria
                            </label>

                            <input
                                name="category"
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Marca / Fabricante
                            </label>

                            <input
                                name="brand"
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Unidade
                            </label>

                            <select
                                name="unit"
                            >

                                <option value="un.">
                                    Unidade
                                </option>

                                <option value="kg">
                                    kg
                                </option>

                                <option value="L">
                                    Litro
                                </option>

                                <option value="balde">
                                    Balde
                                </option>

                                <option value="tambor">
                                    Tambor
                                </option>

                                <option value="caixa">
                                    Caixa
                                </option>

                            </select>

                        </div>


                        <div class="form-group">

                            <label>
                                Estoque atual
                            </label>

                            <input
                                name="stock"
                                type="number"
                                min="0"
                                step="0.01"
                                value="0"
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Estoque mínimo
                            </label>

                            <input
                                name="min"
                                type="number"
                                min="0"
                                step="0.01"
                                value="0"
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Localização
                            </label>

                            <input
                                name="location"
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Lote
                            </label>

                            <input
                                name="lot"
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Validade
                            </label>

                            <input
                                name="expiry"
                                type="date"
                            >

                        </div>


                        <div class="form-group form-full">

                            <label>
                                Informações técnicas
                            </label>

                            <textarea
                                name="technical"
                            ></textarea>

                        </div>


                        <div class="form-group form-full">

                            <label>
                                Observações
                            </label>

                            <textarea
                                name="notes"
                            ></textarea>

                        </div>


                        <div class="form-group form-full">

                            <label>
                                Imagem do produto
                            </label>

                            <input
                                type="file"
                                name="image"
                                accept="image/*"
                            >

                        </div>


                        <div class="form-actions form-full">

                            <button
                                class="btn btn-primary"
                                type="submit"
                            >
                                Salvar produto
                            </button>

                        </div>

                    </div>

                `,

                async function(formData) {

                    const imageFile =
                        formData.get(
                            "image"
                        );


                    let image = "";


                    if (
                        imageFile
                        &&
                        imageFile.size
                    ) {

                        image =
                            await readImage(
                                imageFile
                            );

                    }


                    const product = {

                        id:
                            generateId(),

                        name:
                            formData.get(
                                "name"
                            ),

                        code:
                            formData.get(
                                "code"
                            ),

                        category:
                            formData.get(
                                "category"
                            ),

                        brand:
                            formData.get(
                                "brand"
                            ),

                        unit:
                            formData.get(
                                "unit"
                            ),

                        stock:
                            Number(
                                formData.get(
                                    "stock"
                                )
                            ),

                        min:
                            Number(
                                formData.get(
                                    "min"
                                )
                            ),

                        location:
                            formData.get(
                                "location"
                            ),

                        lot:
                            formData.get(
                                "lot"
                            ),

                        expiry:
                            formData.get(
                                "expiry"
                            ),

                        technical:
                            formData.get(
                                "technical"
                            ),

                        notes:
                            formData.get(
                                "notes"
                            ),

                        image:
                            image

                    };


                    db.products.push(
                        product
                    );


                    saveDatabase();

                }

            );

        };

}


/* =========================================================
   MATERIAIS DE EXPEDIÇÃO
========================================================= */

if ($("newMaterialBtn")) {

    $("newMaterialBtn")
        .onclick =
        function() {

            openModal(

                "Cadastrar material de expedição",

                `

                    <div class="form-grid">

                        <div class="form-group">

                            <label>
                                Material
                            </label>

                            <input
                                name="name"
                                placeholder="Ex.: Caixa média"
                                required
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Tipo
                            </label>

                            <select
                                name="type"
                            >

                                <option>
                                    Caixinha
                                </option>

                                <option>
                                    Caixa numerada
                                </option>

                                <option>
                                    Fita
                                </option>

                                <option>
                                    Etiqueta
                                </option>

                                <option>
                                    Envelope
                                </option>

                                <option>
                                    Plástico
                                </option>

                                <option>
                                    Outro
                                </option>

                            </select>

                        </div>


                        <div class="form-group">

                            <label>
                                Estoque atual
                            </label>

                            <input
                                name="stock"
                                type="number"
                                min="0"
                                step="1"
                                value="0"
                                required
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Estoque mínimo
                            </label>

                            <input
                                name="min"
                                type="number"
                                min="0"
                                step="1"
                                value="0"
                            >

                        </div>


                        <div class="form-group">

                            <label>
                                Unidade
                            </label>

                            <select
                                name="unit"
                            >

                                <option value="un.">
                                    Unidade
                                </option>

                                <option value="pacote">
                                    Pacote
                                </option>

                                <option value="rolo">
                                    Rolo
                                </option>

                                <option value="caixa">
                                    Caixa
                                </option>

                            </select>

                        </div>


                        <div class="form-group form-full">

                            <label>
                                Observações
                            </label>

                            <textarea
                                name="notes"
                            ></textarea>

                        </div>


                        <div class="form-actions form-full">

                            <button
                                class="btn btn-primary"
                                type="submit"
                            >
                                Salvar material
                            </button>

                        </div>

                    </div>

                `,

                function(formData) {

                    db.materials.push({

                        id:
                            generateId(),

                        name:
                            formData.get(
                                "name"
                            ),

                        type:
                            formData.get(
                                "type"
                            ),

                        stock:
                            Number(
                                formData.get(
                                    "stock"
                                )
                            ),

                        min:
                            Number(
                                formData.get(
                                    "min"
                                )
                            ),

                        unit:
                            formData.get(
                                "unit"
                            ),

                        notes:
                            formData.get(
                                "notes"
                            )

                    });


                    saveDatabase();

                }

            );

        };

}


/* =========================================================
   RENDERIZAR MATERIAIS
========================================================= */

function renderMaterials() {

    const table =
        $("materialsTable");


    if (!table) {
        return;
    }


    const search =
        (
            $("materialSearch")?.value
            ||
            ""
        )
        .toLowerCase()
        .trim();


    const materials =
        db.materials.filter(

            function(material) {

                return (

                    (
                        material.name
                        ||
                        ""
                    )
                    +
                    " "
                    +
                    (
                        material.type
                        ||
                        ""
                    )

                )
                .toLowerCase()
                .includes(
                    search
                );

            }

        );


    if (!materials.length) {

        table.innerHTML = `

            <tr>

                <td
                    colspan="7"
                    class="empty"
                >
                    Nenhum material cadastrado.
                </td>

            </tr>

        `;

        return;

    }


    table.innerHTML =

        materials.map(

            function(material) {

                const status =
                    getStatus(
                        Number(
                            material.stock
                            ||
                            0
                        ),
                        Number(
                            material.min
                            ||
                            0
                        )
                    );


                return `

                    <tr>

                        <td>
                            ${escapeHTML(
                                material.name
                            )}
                        </td>

                        <td>
                            <span
                                class="material-type"
                            >
                                ${escapeHTML(
                                    material.type
                                    ||
                                    ""
                                )}
                            </span>
                        </td>

                        <td>
                            ${formatNumber(
                                material.stock
                            )}
                        </td>

                        <td>
                            ${formatNumber(
                                material.min
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                material.unit
                                ||
                                "un."
                            )}
                        </td>

                        <td>

                            <span
                                class="status ${status.className}"
                            >
                                ${status.text}
                            </span>

                        </td>

                        <td>

                            <button
                                class="delete-btn"
                                onclick="deleteMaterial('${material.id}')"
                            >
                                Excluir
                            </button>

                        </td>

                    </tr>

                `;

            }

        ).join("");

}


/* =========================================================
   SELECTS
========================================================= */

function renderSelects() {

    const productSelect =
        $("entryProduct");


    if (productSelect) {

        productSelect.innerHTML = `

            <option value="">
                Selecione o produto
            </option>

            ${
                db.products
                    .map(

                        function(product) {

                            return `

                                <option
                                    value="${product.id}"
                                >
                                    ${escapeHTML(
                                        product.name
                                    )}
                                    —
                                    estoque:
                                    ${formatNumber(
                                        product.stock
                                    )}
                                </option>

                            `;

                        }

                    )
                    .join("")
            }

        `;

    }


    const exitProduct =
        $("exitProduct");


    if (exitProduct) {

        exitProduct.innerHTML = `

            <option value="">
                Selecione o produto
            </option>

            ${
                db.products
                    .map(

                        function(product) {

                            return `

                                <option
                                    value="${product.id}"
                                >
                                    ${escapeHTML(
                                        product.name
                                    )}
                                    —
                                    estoque:
                                    ${formatNumber(
                                        product.stock
                                    )}
                                </option>

                            `;

                        }

                    )
                    .join("")
            }

        `;

    }


    const clientSelect =
        $("exitClient");


    if (clientSelect) {

        clientSelect.innerHTML = `

            <option value="">
                Selecione o cliente
            </option>

            ${
                db.clients
                    .map(

                        function(client) {

                            return `

                                <option
                                    value="${client.id}"
                                >
                                    ${escapeHTML(
                                        client.name
                                    )}
                                </option>

                            `;

                        }

                    )
                    .join("")
            }

        `;

    }

}


/* =========================================================
   SELECT DE CAIXAS PARA VENDAS
========================================================= */

function renderSaleBoxSelect() {

    const select =
        $("saleBox");


    if (!select) {
        return;
    }


    const boxes =
        db.materials.filter(

            function(material) {

                const type =
                    String(
                        material.type
                        ||
                        ""
                    )
                    .toLowerCase();


                return (
                    type.includes(
                        "caixa"
                    )
                    ||
                    type.includes(
                        "caixinha"
                    )
                );

            }

        );


    select.innerHTML = `

        <option value="">
            Selecione a caixa utilizada
        </option>

        ${
            boxes.map(

                function(box) {

                    return `

                        <option
                            value="${box.id}"
                        >
                            ${escapeHTML(
                                box.name
                            )}
                            —
                            estoque:
                            ${formatNumber(
                                box.stock
                            )}
                        </option>

                    `;

                }

            ).join("")
        }

    `;

}


/* =========================================================
   FORMULÁRIOS ANTIGOS
========================================================= */

function setupForms() {

    /* =====================================================
       ENTRADA
    ===================================================== */

    if ($("entryForm")) {

        $("entryForm")
            .addEventListener(
                "submit",
                function(event) {

                    event.preventDefault();


                    const formData =
                        new FormData(
                            event.target
                        );


                    const product =
                        db.products.find(

                            function(item) {

                                return (
                                    item.id
                                    ===
                                    formData.get(
                                        "product"
                                    )
                                );

                            }

                        );


                    const qty =
                        Number(
                            formData.get(
                                "qty"
                            )
                        );


                    if (!product) {

                        alert(
                            "Selecione um produto."
                        );

                        return;

                    }


                    if (
                        !qty
                        ||
                        qty <= 0
                    ) {

                        alert(
                            "Informe uma quantidade válida."
                        );

                        return;

                    }


                    product.stock =
                        Number(
                            product.stock
                            ||
                            0
                        )
                        +
                        qty;


                    db.movements.push({

                        id:
                            generateId(),

                        type:
                            "entry",

                        item:
                            product.name,

                        productId:
                            product.id,

                        qty:
                            qty,

                        date:
                            formData.get(
                                "date"
                            ),

                        party:
                            formData.get(
                                "supplier"
                            )
                            ||
                            "",

                        user:
                            currentUser,

                        notes:
                            formData.get(
                                "notes"
                            )
                            ||
                            ""

                    });


                    saveDatabase();


                    event.target.reset();


                    if ($("entryDate")) {

                        $("entryDate")
                            .value =
                            today();

                    }


                    renderAll();


                    alert(
                        "Entrada registrada com sucesso."
                    );

                }
            );

    }


    /* =====================================================
       SAÍDA
    ===================================================== */

    if ($("exitForm")) {

        $("exitForm")
            .addEventListener(
                "submit",
                function(event) {

                    event.preventDefault();


                    const formData =
                        new FormData(
                            event.target
                        );


                    const product =
                        db.products.find(

                            function(item) {

                                return (
                                    item.id
                                    ===
                                    formData.get(
                                        "product"
                                    )
                                );

                            }

                        );


                    const qty =
                        Number(
                            formData.get(
                                "qty"
                            )
                        );


                    if (!product) {

                        alert(
                            "Selecione um produto."
                        );

                        return;

                    }


                    if (
                        !qty
                        ||
                        qty <= 0
                    ) {

                        alert(
                            "Informe uma quantidade válida."
                        );

                        return;

                    }


                    if (
                        qty
                        >
                        Number(
                            product.stock
                            ||
                            0
                        )
                    ) {

                        alert(
                            "Quantidade maior que o estoque disponível."
                        );

                        return;

                    }


                    product.stock -=
                        qty;


                    db.movements.push({

                        id:
                            generateId(),

                        type:
                            "exit",

                        item:
                            product.name,

                        productId:
                            product.id,

                        qty:
                            qty,

                        date:
                            formData.get(
                                "date"
                            ),

                        party:
                            formData.get(
                                "client"
                            )
                            ||
                            "",

                        user:
                            currentUser,

                        notes:
                            formData.get(
                                "notes"
                            )
                            ||
                            ""

                    });


                    saveDatabase();


                    event.target.reset();


                    if ($("exitDate")) {

                        $("exitDate")
                            .value =
                            today();

                    }


                    renderAll();


                    alert(
                        "Saída registrada com sucesso."
                    );

                }
            );

    }


    /* =====================================================
       NOVO CLIENTE
    ===================================================== */

    if ($("newClientBtn")) {

        $("newClientBtn")
            .onclick =
            function() {

                openModal(

                    "Cadastrar cliente",

                    `

                        <div class="form-grid">

                            <div class="form-group">

                                <label>
                                    Nome / Razão social
                                </label>

                                <input
                                    name="name"
                                    required
                                >

                            </div>


                            <div class="form-group">

                                <label>
                                    CPF / CNPJ
                                </label>

                                <input
                                    name="document"
                                >

                            </div>


                            <div class="form-group">

                                <label>
                                    Telefone
                                </label>

                                <input
                                    name="phone"
                                >

                            </div>


                            <div class="form-group">

                                <label>
                                    E-mail
                                </label>

                                <input
                                    name="email"
                                    type="email"
                                >

                            </div>


                            <div class="form-group form-full">

                                <label>
                                    Endereço
                                </label>

                                <input
                                    name="address"
                                >

                            </div>


                            <div class="form-group form-full">

                                <label>
                                    Observações
                                </label>

                                <textarea
                                    name="notes"
                                ></textarea>

                            </div>


                            <div class="form-actions form-full">

                                <button
                                    class="btn btn-primary"
                                    type="submit"
                                >
                                    Salvar cliente
                                </button>

                            </div>

                        </div>

                    `,

                    function(formData) {

                        db.clients.push({

                            id:
                                generateId(),

                            name:
                                formData.get(
                                    "name"
                                ),

                            document:
                                formData.get(
                                    "document"
                                ),

                            phone:
                                formData.get(
                                    "phone"
                                ),

                            email:
                                formData.get(
                                    "email"
                                ),

                            address:
                                formData.get(
                                    "address"
                                ),

                            notes:
                                formData.get(
                                    "notes"
                                )

                        });


                        saveDatabase();

                    }

                );

            };

    }

}


/* =========================================================
   CLIENTES
========================================================= */

function renderClients() {

    const table =
        $("clientsTable");


    if (!table) {
        return;
    }


    if (!db.clients.length) {

        table.innerHTML = `

            <tr>

                <td
                    colspan="6"
                    class="empty"
                >
                    Nenhum cliente cadastrado.
                </td>

            </tr>

        `;

        return;

    }


    table.innerHTML =

        db.clients.map(

            function(client) {

                return `

                    <tr>

                        <td>

                            <strong>
                                ${escapeHTML(
                                    client.name
                                )}
                            </strong>

                        </td>

                        <td>
                            ${escapeHTML(
                                client.document
                                ||
                                "—"
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                client.phone
                                ||
                                "—"
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                client.email
                                ||
                                "—"
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                client.address
                                ||
                                "—"
                            )}
                        </td>

                        <td>

                            <button
                                class="delete-btn"
                                onclick="deleteClient('${client.id}')"
                            >
                                Excluir
                            </button>

                        </td>

                    </tr>

                `;

            }

        ).join("");

}


/* =========================================================
   HISTÓRICO
========================================================= */

function renderHistory() {

    const table =
        $("historyTable");


    if (!table) {
        return;
    }


    const movements =
        [...db.movements]
            .reverse();


    if (!movements.length) {

        table.innerHTML = `

            <tr>

                <td
                    colspan="6"
                    class="empty"
                >
                    Nenhuma movimentação registrada.
                </td>

            </tr>

        `;

        return;

    }


    table.innerHTML =

        movements.map(

            function(movement) {

                let type =
                    "Saída";


                let className =
                    "movement-exit";


                if (
                    movement.type
                    ===
                    "entry"
                ) {

                    type =
                        "Entrada";

                    className =
                        "movement-entry";

                }


                if (
                    movement.type
                    ===
                    "sale"
                ) {

                    type =
                        "Venda / Nota";

                    className =
                        "movement-sale";

                }


                if (
                    movement.type
                    ===
                    "box_exit"
                ) {

                    type =
                        "Baixa de Caixa";

                    className =
                        "movement-exit";

                }


                return `

                    <tr>

                        <td>
                            ${escapeHTML(
                                movement.date
                                ||
                                ""
                            )}
                        </td>

                        <td>

                            <span
                                class="movement-badge ${className}"
                            >
                                ${type}
                            </span>

                        </td>

                        <td>
                            ${escapeHTML(
                                movement.item
                                ||
                                ""
                            )}
                        </td>

                        <td>
                            ${formatNumber(
                                movement.qty
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                movement.party
                                ||
                                "—"
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                movement.user
                                ||
                                ""
                            )}
                        </td>

                    </tr>

                `;

            }

        ).join("");

}


/* =========================================================
   NOTAS / VENDAS
========================================================= */

function setupSalesForms() {

    if (!$("saleForm")) {
        return;
    }


    $("saleForm")
        .addEventListener(
            "submit",
            function(event) {

                event.preventDefault();


                const formData =
                    new FormData(
                        event.target
                    );


                const product =
                    db.products.find(

                        function(item) {

                            return (
                                item.id
                                ===
                                formData.get(
                                    "product"
                                )
                            );

                        }

                    );


                const box =
                    db.materials.find(

                        function(item) {

                            return (
                                item.id
                                ===
                                formData.get(
                                    "box"
                                )
                            );

                        }

                    );


                const qty =
                    Number(
                        formData.get(
                            "qty"
                        )
                    );


                /* =========================================
                   VALIDAÇÕES
                ========================================= */

                if (!formData.get("date")) {

                    alert(
                        "Informe a data da nota."
                    );

                    return;

                }


                if (!product) {

                    alert(
                        "Selecione o produto."
                    );

                    return;

                }


                if (!box) {

                    alert(
                        "Selecione a caixa utilizada."
                    );

                    return;

                }


                if (
                    !qty
                    ||
                    qty <= 0
                ) {

                    alert(
                        "Informe uma quantidade válida."
                    );

                    return;

                }


                if (
                    qty
                    >
                    Number(
                        product.stock
                        ||
                        0
                    )
                ) {

                    alert(
                        "A quantidade do produto é maior que o estoque atual."
                    );

                    return;

                }


                if (
                    Number(
                        box.stock
                        ||
                        0
                    )
                    <
                    1
                ) {

                    alert(
                        "A caixa selecionada está sem estoque."
                    );

                    return;

                }


                /* =========================================
                   BAIXA DO PRODUTO
                ========================================= */

                product.stock =
                    Number(
                        product.stock
                        ||
                        0
                    )
                    -
                    qty;


                /* =========================================
                   BAIXA DA CAIXA
                ========================================= */

                box.stock =
                    Number(
                        box.stock
                        ||
                        0
                    )
                    -
                    1;


                /* =========================================
                   REGISTRO DA VENDA
                ========================================= */

                const sale = {

                    id:
                        generateId(),

                    type:
                        "sale",

                    date:
                        formData.get(
                            "date"
                        ),

                    invoice:
                        formData.get(
                            "invoice"
                        )
                        ||
                        "",

                    platform:
                        formData.get(
                            "platform"
                        )
                        ||
                        "",

                    order:
                        formData.get(
                            "order"
                        )
                        ||
                        "",

                    client:
                        formData.get(
                            "client"
                        )
                        ||
                        "",

                    productId:
                        product.id,

                    productName:
                        product.name,

                    qty:
                        qty,

                    boxId:
                        box.id,

                    boxName:
                        box.name,

                    boxNumber:
                        formData.get(
                            "boxNumber"
                        )
                        ||
                        "",

                    notes:
                        formData.get(
                            "notes"
                        )
                        ||
                        "",

                    user:
                        currentUser

                };


                db.sales.push(
                    sale
                );


                /* =========================================
                   HISTÓRICO DO PRODUTO
                ========================================= */

                db.movements.push({

                    id:
                        generateId(),

                    type:
                        "sale",

                    item:
                        product.name,

                    productId:
                        product.id,

                    qty:
                        qty,

                    date:
                        sale.date,

                    party:
                        sale.client,

                    user:
                        currentUser,

                    invoice:
                        sale.invoice,

                    order:
                        sale.order,

                    platform:
                        sale.platform,

                    boxName:
                        box.name,

                    boxNumber:
                        sale.boxNumber,

                    notes:
                        sale.notes

                });


                /* =========================================
                   HISTÓRICO DA CAIXA
                ========================================= */

                db.movements.push({

                    id:
                        generateId(),

                    type:
                        "box_exit",

                    item:
                        box.name,

                    materialId:
                        box.id,

                    qty:
                        1,

                    date:
                        sale.date,

                    party:
                        sale.client,

                    user:
                        currentUser,

                    invoice:
                        sale.invoice,

                    order:
                        sale.order,

                    boxNumber:
                        sale.boxNumber,

                    notes:
                        "Caixa utilizada na expedição"

                });


                saveDatabase();


                event.target.reset();


                if ($("saleDate")) {

                    $("saleDate")
                        .value =
                        today();

                }


                renderAll();


                alert(
                    "Nota registrada! Produto e caixa foram baixados do estoque."
                );

            }

        );

}


/* =========================================================
   RENDERIZAR PRODUTOS E CAIXAS NA VENDA
========================================================= */

function renderSaleProductSelect() {

    const select =
        $("saleProduct");


    if (!select) {
        return;
    }


    select.innerHTML = `

        <option value="">
            Selecione o produto
        </option>

        ${
            db.products.map(

                function(product) {

                    return `

                        <option
                            value="${product.id}"
                        >

                            ${escapeHTML(
                                product.name
                            )}

                            —
                            estoque:
                            ${formatNumber(
                                product.stock
                            )}

                        </option>

                    `;

                }

            ).join("")
        }

    `;

}


function renderSaleBoxSelect() {

    const select =
        $("saleBox");


    if (!select) {
        return;
    }


    const boxes =
        db.materials.filter(

            function(material) {

                const type =
                    String(
                        material.type
                        ||
                        ""
                    )
                    .toLowerCase();


                return (
                    type.includes(
                        "caixa"
                    )
                    ||
                    type.includes(
                        "caixinha"
                    )
                );

            }

        );


    select.innerHTML = `

        <option value="">
            Selecione a caixa utilizada
        </option>

        ${
            boxes.map(

                function(box) {

                    return `

                        <option
                            value="${box.id}"
                        >

                            ${escapeHTML(
                                box.name
                            )}

                            —
                            estoque:
                            ${formatNumber(
                                box.stock
                            )}

                        </option>

                    `;

                }

            ).join("")
        }

    `;

}


/* =========================================================
   RENDERIZAR VENDAS
========================================================= */

function renderSales() {

    const table =
        $("salesTable");


    if (!table) {
        return;
    }


    const sales =
        [...db.sales]
            .reverse();


    if (!sales.length) {

        table.innerHTML = `

            <tr>

                <td
                    colspan="10"
                    class="empty"
                >
                    Nenhuma venda registrada.
                </td>

            </tr>

        `;

        return;

    }


    table.innerHTML =

        sales.map(

            function(sale) {

                return `

                    <tr>

                        <td>
                            ${escapeHTML(
                                sale.date
                                ||
                                ""
                            )}
                        </td>

                        <td>

                            <strong>
                                ${escapeHTML(
                                    sale.invoice
                                    ||
                                    "—"
                                )}
                            </strong>

                        </td>

                        <td>

                            <span
                                class="sale-platform"
                            >
                                ${escapeHTML(
                                    sale.platform
                                    ||
                                    "—"
                                )}
                            </span>

                        </td>

                        <td>
                            ${escapeHTML(
                                sale.order
                                ||
                                "—"
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                sale.client
                                ||
                                "—"
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                sale.productName
                            )}
                        </td>

                        <td>
                            ${formatNumber(
                                sale.qty
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                sale.boxName
                                ||
                                "—"
                            )}
                        </td>

                        <td>

                            ${
                                sale.boxNumber
                                ?

                                `
                                    <span
                                        class="box-badge"
                                    >
                                        Nº
                                        ${escapeHTML(
                                            sale.boxNumber
                                        )}
                                    </span>
                                `

                                :

                                "—"
                            }

                        </td>

                        <td>
                            ${escapeHTML(
                                sale.user
                                ||
                                ""
                            )}
                        </td>

                    </tr>

                `;

            }

        ).join("");

}


/* =========================================================
   NOTAS DIÁRIAS
========================================================= */

function setupDailyNotes() {

    if (!$("dailyNoteForm")) {
        return;
    }


    if (
        $("dailyNoteForm")
        .dataset
        .configured
        ===
        "true"
    ) {

        return;

    }


    $("dailyNoteForm")
        .dataset
        .configured =
        "true";


    $("dailyNoteForm")
        .addEventListener(
            "submit",
            function(event) {

                event.preventDefault();


                const formData =
                    new FormData(
                        event.target
                    );


                const text =
                    String(
                        formData.get(
                            "text"
                        )
                        ||
                        ""
                    )
                    .trim();


                if (!text) {

                    alert(
                        "Digite a anotação diária."
                    );

                    return;

                }


                db.dailyNotes.push({

                    id:
                        generateId(),

                    date:
                        formData.get(
                            "date"
                        ),

                    text:
                        text,

                    user:
                        currentUser

                });


                saveDatabase();


                event.target.reset();


                if ($("dailyNoteDate")) {

                    $("dailyNoteDate")
                        .value =
                        today();

                }


                renderDailyNotes();

            }

        );

}


function renderDailyNotes() {

    const container =
        $("dailyNotesList");


    if (!container) {
        return;
    }


    const notes =
        [...db.dailyNotes]
            .reverse();


    if (!notes.length) {

        container.innerHTML = `

            <div class="empty">

                Nenhuma anotação diária registrada.

            </div>

        `;

        return;

    }


    container.innerHTML =

        notes.map(

            function(note) {

                return `

                    <div
                        class="daily-note"
                    >

                        <div
                            class="daily-note-header"
                        >

                            <div>

                                <strong>
                                    ${escapeHTML(
                                        note.date
                                    )}
                                </strong>

                                <span>
                                    —
                                    ${escapeHTML(
                                        note.user
                                    )}
                                </span>

                            </div>

                            <button
                                class="delete-btn"
                                onclick="deleteDailyNote('${note.id}')"
                            >
                                Excluir
                            </button>

                        </div>

                        <div
                            class="daily-note-text"
                        >
                            ${escapeHTML(
                                note.text
                            )}
                        </div>

                    </div>

                `;

            }

        ).join("");

}


function deleteDailyNote(id) {

    if (
        !confirm(
            "Deseja excluir esta anotação?"
        )
    ) {

        return;

    }


    db.dailyNotes =
        db.dailyNotes.filter(

            function(note) {

                return note.id !== id;

            }

        );


    saveDatabase();

    renderDailyNotes();

}


/* =========================================================
   OCORRÊNCIAS
========================================================= */

function setupOccurrenceForms() {

    setupDailyNotes();


    if (!$("occurrenceForm")) {
        return;
    }


    if (
        $("occurrenceForm")
            .dataset
            .configured
        ===
        "true"
    ) {

        return;

    }


    $("occurrenceForm")
        .dataset
        .configured =
        "true";


    $("occurrenceForm")
        .addEventListener(
            "submit",
            function(event) {

                event.preventDefault();


                const formData =
                    new FormData(
                        event.target
                    );


                const description =
                    String(
                        formData.get(
                            "description"
                        )
                        ||
                        ""
                    )
                    .trim();


                if (!description) {

                    alert(
                        "Descreva a ocorrência."
                    );

                    return;

                }


                db.occurrences.push({

                    id:
                        generateId(),

                    type:
                        formData.get(
                            "type"
                        ),

                    date:
                        formData.get(
                            "date"
                        ),

                    order:
                        formData.get(
                            "order"
                        )
                        ||
                        "",

                    invoice:
                        formData.get(
                            "invoice"
                        )
                        ||
                        "",

                    client:
                        formData.get(
                            "client"
                        )
                        ||
                        "",

                    tracking:
                        formData.get(
                            "tracking"
                        )
                        ||
                        "",

                    description:
                        description,

                    status:
                        formData.get(
                            "status"
                        )
                        ||
                        "aberto",

                    user:
                        currentUser

                });


                saveDatabase();


                event.target.reset();


                if ($("occurrenceDate")) {

                    $("occurrenceDate")
                        .value =
                        today();

                }


                renderOccurrences();

            }

        );

}


function occurrenceTypeLabel(
    type
) {

    return {

        wrong_order:
            "Pedido errado",

        open_service:
            "Atendimento em aberto",

        melhor_envio:
            "Melhor Envio"

    }[
        type
    ]
    ||
    type
    ||
    "";

}


function occurrenceStatusLabel(
    status
) {

    return {

        aberto:
            "Aberto",

        em_andamento:
            "Em andamento",

        resolvido:
            "Resolvido"

    }[
        status
    ]
    ||
    status
    ||
    "";

}


function occurrenceStatusClass(
    status
) {

    if (
        status
        ===
        "resolvido"
    ) {

        return "status-ok";

    }


    if (
        status
        ===
        "em_andamento"
    ) {

        return "status-low";

    }


    return "status-out";

}


function renderOccurrences() {

    const table =
        $("occurrencesTable");


    if (!table) {
        return;
    }


    const occurrences =
        [...db.occurrences]
            .reverse();


    if (!occurrences.length) {

        table.innerHTML = `

            <tr>

                <td
                    colspan="10"
                    class="empty"
                >
                    Nenhuma ocorrência registrada.
                </td>

            </tr>

        `;

        return;

    }


    table.innerHTML =

        occurrences.map(

            function(item) {

                return `

                    <tr>

                        <td>
                            ${escapeHTML(
                                item.date
                            )}
                        </td>

                        <td>

                            <span
                                class="occurrence-type"
                            >

                                ${escapeHTML(
                                    occurrenceTypeLabel(
                                        item.type
                                    )
                                )}

                            </span>

                        </td>

                        <td>
                            ${escapeHTML(
                                item.order
                                ||
                                "—"
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                item.invoice
                                ||
                                "—"
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                item.client
                                ||
                                "—"
                            )}
                        </td>

                        <td>

                            ${
                                item.tracking

                                ?

                                `
                                    <span
                                        class="tracking-code"
                                    >
                                        ${escapeHTML(
                                            item.tracking
                                        )}
                                    </span>
                                `

                                :

                                "—"
                            }

                        </td>

                        <td>

                            <span
                                class="status ${occurrenceStatusClass(
                                    item.status
                                )}"
                            >

                                ${escapeHTML(
                                    occurrenceStatusLabel(
                                        item.status
                                    )
                                )}

                            </span>

                        </td>

                        <td>
                            ${escapeHTML(
                                item.description
                            )}
                        </td>

                        <td>
                            ${escapeHTML(
                                item.user
                            )}
                        </td>

                        <td>

                            <button
                                class="delete-btn"
                                onclick="deleteOccurrence('${item.id}')"
                            >
                                Excluir
                            </button>

                        </td>

                    </tr>

                `;

            }

        ).join("");

}


function deleteOccurrence(id) {

    if (
        !confirm(
            "Deseja excluir esta ocorrência?"
        )
    ) {

        return;

    }


    db.occurrences =
        db.occurrences.filter(

            function(item) {

                return item.id !== id;

            }

        );


    saveDatabase();

    renderOccurrences();

}


/* =========================================================
   EXCLUSÕES
========================================================= */

function deleteProduct(id) {

    if (!canEditProducts()) {

        alert(
            "Seu acesso é somente para leitura dos produtos."
        );

        return;

    }


    const confirmed =
        confirm(
            "Deseja realmente excluir este produto?"
        );


    if (!confirmed) {
        return;
    }


    db.products =
        db.products.filter(

            function(product) {

                return product.id !== id;

            }

        );


    saveDatabase();

    renderAll();

}


function deleteMaterial(id) {

    const confirmed =
        confirm(
            "Deseja realmente excluir este material?"
        );


    if (!confirmed) {
        return;
    }


    db.materials =
        db.materials.filter(

            function(material) {

                return material.id !== id;

            }

        );


    saveDatabase();

    renderAll();

}


function deleteClient(id) {

    const confirmed =
        confirm(
            "Deseja realmente excluir este cliente?"
        );


    if (!confirmed) {
        return;
    }


    db.clients =
        db.clients.filter(

            function(client) {

                return client.id !== id;

            }

        );


    saveDatabase();

    renderAll();

}


/* =========================================================
   PESQUISAS
========================================================= */

function setupSearches() {

    if ($("productSearch")) {

        $("productSearch")
            .addEventListener(
                "input",
                function() {

                    renderProducts();

                }
            );

    }


    if ($("materialSearch")) {

        $("materialSearch")
            .addEventListener(
                "input",
                function() {

                    renderMaterials();

                }
            );

    }

}


/* =========================================================
   LOGOUT
========================================================= */

if ($("logoutBtn")) {

    $("logoutBtn")
        .addEventListener(
            "click",
            function() {

                sessionStorage.removeItem(
                    "unilub_user"
                );


                location.reload();

            }
        );

}


/* =========================================================
   CORREÇÃO DOS SELECTS DE VENDAS
========================================================= */

function renderSaleSelects() {

    renderSaleProductSelect();

    renderSaleBoxSelect();

}


/* =========================================================
   SOBRESCREVER RENDER ALL COM SELECTS COMPLETOS
========================================================= */

const originalRenderAll =
    renderAll;


renderAll =
    function() {

        renderDashboard();

        renderProducts();

        renderMaterials();

        renderSelects();

        renderClients();

        renderHistory();

        renderSaleProductSelect();

        renderSaleBoxSelect();

        renderSales();

        renderDailyNotes();

        renderOccurrences();

    };


/* =========================================================
   INICIAR SISTEMA
========================================================= */

setupLogin();


if (currentUser) {

    if ($("loginScreen")) {

        $("loginScreen")
            .classList
            .add("hidden");

    }


    if ($("app")) {

        $("app")
            .classList
            .remove("hidden");

    }


    initializeApp();

}
