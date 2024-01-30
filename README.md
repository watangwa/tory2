PJS.isProd = window.location.hostname === 'www.toryburch.com';
PJS.isStaging = window.location.hostname === 'com.stg.aem.toryburch.com';
PJS.isUAT = window.location.hostname === 'com.uat.aem.toryburch.com';
PJS.isPreview = window.location.hostname === 'na.preview.prod.aem.toryburch.com';
PJS.isCheckout = window.location.pathname.includes('/checkout/');
PJS.isHomepage = window.location.pathname === '/en-us/' || window.location.pathname === '/';
PJS.isPDP = window.location.pathname.includes('.html');

/**
 * Cro Metrics Utilities
 * @module general/cro-metrics-utilities.Optimizely.module
 */

/**
 * Detect Async Optimizely
 * @module general/detect-async.module
 */

/**
 * change to spa/history-state-change.module when code approved by TB
 * (and archive local version)
 * Watch History State Change (for checkout)
 * @module history-state-change.module
 */

/**
 * Bot Filtering Module
 * @module userAgent_attribute.module
 */

/**
 * Initialize Feature Flag integration
 * @module initFeatureFlag.module
 */

if (PJS.mode === 'qa') {
  /**
   * Track Experiment Revenue
   * @module revenue-tracking-WIP.module
   */
  
} else {
  /**
   * Track Experiment Revenue
   * @module revenue-tracking
   */
}

/**
 * Page View Attibutes
 * @module pageViewAttributes.module
 */

if (PJS.isHomepage || PJS.isPDP) {
  /**
   * Page Scroll events
   * @module optimizely/page-scroll-events.module
   */
}

// Modules that are not required to run in the checkout flow
if (!PJS.isCheckout) {
  /**
   * New vs Returning Attribute
   * @module optimizely/returning-visitors-segmentation.module
   */

  if (PJS.mode === 'qa') {
     /**
     * Track Add to Cart Events
     * @module add-to-cart-tracking-WIP.module
     */
    
  } else {
    /**
     * Track Add to Cart Events
     * @module add-to-cart-tracking.module
     */
  }
  /**
   * Page/product specific ATC
   * @module individual_add_to_cart.module
   */

  if (PJS.mode === 'qa') {
    /**
   * Set up Recommender Util
   * @module recommender-utils.module
   */

    /**
   * User Behavior tracking module
   * @module account_tracking_WIP.module
   */

  } else {
    /**
   * Set up Recommender Util
   * @module recommender-utils
   */

    /**
   * User Behavior tracking module
   * @module account_tracking-broken.module
   */
    
  }
}
account_tracking-broken.module.js
try{
!function(){
// ../../_includes/cromedics/cookies.ts
function setCookie(name, value, {
  duration,
  domain = window.location.hostname.split(".").slice(-2).join("."),
  sameSite = "Lax"
} = {}) {
  let cookie = `${name}=${value}; Path=/`;
  let date;
  if (duration instanceof Date) {
    date = duration;
  } else if (duration > 0) {
    const hours = duration * 60 * 60 * 1e3;
    date = new Date();
    date.setTime(date.getTime() + hours);
  }
  if (date)
    cookie += `; Expires=${date.toUTCString()}`;
  if (domain)
    cookie += `; Domain=${domain}`;
  cookie += `; SameSite=${sameSite}`;
  if (sameSite.toLowerCase() === "none")
    cookie += `; Secure;`;
  document.cookie = cookie;
}

// ../../_includes/oli/optimizely/lifecycle.ts
function waitForUtils(callback) {
  onInitialized(() => {
    callback(window.optimizely.get("utils"));
  });
}
function onInitialized(callback) {
  window.optimizely.initialized && callback() || window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "initialized" },
    handler: callback
  });
}

// ../../_includes/oli/optimizely/events.ts
var sendEvent = (eventName, tags = {}) => {
  window.optimizely.push({
    type: "event",
    eventName,
    tags
  });
};

// @oli:logs:@oli:logs:account_tracking-broken.module
var log = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[account_tracking-broken.module]"))();
var error2 = /* @__PURE__ */ (() => window.CRO_PJS.error.bind(window, "account_tracking-broken.module"))();

// ../../_includes/cromedics/monkey-patch.ts
function monkeyPatchBefore(object, key, callback) {
  const original = object[key];
  object[key] = function(...args) {
    try {
      callback(...args);
    } catch (e) {
      console.error(e);
    }
    return typeof original === "function" && original.apply(this, args);
  };
}

// ../../_includes/cromedics/datalayer.ts
var waitForDataLayer = (dataLayerKey = "dataLayer") => new Promise((resolve) => {
  (function poll() {
    if (!window[dataLayerKey] || typeof window[dataLayerKey].push !== "function")
      return setTimeout(poll, 50);
    return resolve(window[dataLayerKey]);
  })();
});
var onDataLayerPush = (callback, {
  dataLayerKey = "dataLayer",
  withPastData = true
} = {}) => waitForDataLayer(dataLayerKey).then((dataLayer) => {
  if (withPastData) {
    dataLayer.forEach((data) => {
      callback(data);
    });
  }
  monkeyPatchBefore(dataLayer, "push", callback);
});

// ../../_includes/oli/optimizely/attributes.ts
var setAttributes = (attributes, eventName = "push_attribute") => {
  window.optimizely.push({ type: "user", attributes });
  window.optimizely.push({ type: "event", eventName });
};
var setDefaultAttributes = (defaultAttributes, eventName = "push_attribute") => {
  const visitor = window.optimizely.get("visitor");
  const currentAttributes = Object.values(visitor.custom || {});
  const attributes = Object.entries(defaultAttributes).reduce((out, [key, value]) => {
    if (!currentAttributes.find((attr) => attr.id === key || attr.name === key))
      out[key] = value;
    return out;
  }, {});
  if (Object.keys(attributes).length) {
    setAttributes(attributes, eventName);
  }
};

// _projectjs/account_tracking-broken.module.ts
function account_tracking_broken_module_default() {
  waitForUtils((utils) => {
    setDefaultAttributes({ pjs_tb_api_returning_customer: "Unknown" });
    if (window.location.search.includes("?")) {
      const parameters = window.location.search.slice(1).split("&");
      parameters.forEach((thisParam) => {
        const lowerCaseParam = thisParam.toLowerCase();
        if (lowerCaseParam.includes("utm_") || lowerCaseParam.includes("gclsrc") || lowerCaseParam.includes("journey")) {
          const source = thisParam.split("=");
          const cookieValue = typeof source[1] === "string" ? source[1].toLowerCase() : source[1];
          setCookie(source[0], cookieValue);
        }
      });
    }
    if (window.location.pathname.includes("gated-springevent")) {
      setAttributes({ pjs_viewed_spring_event: "true" });
      setCookie(`pjs_viewed_spring_event`, `true`);
    }
    utils.waitForElement(`[data-id="AuthPortal"] [data-id="SignInForm"] button[data-id="SignInFormSubmitButton"]`).then((elSignInButton) => {
      const accountLink = document.querySelector(`[data-id="MyAccountLink"]`);
      if (accountLink) {
        elSignInButton.addEventListener("click", () => {
          const accountFound = utils.waitUntil(() => accountLink.querySelector(`div[class^="element__text"]`) != void 0 && accountLink.querySelector(`div[class^="element__text"]`)?.textContent != "Sign in");
          accountFound.then(() => {
            sendEvent(`pjs_sign_in_completed`);
          }).catch(error2);
        }, { once: true });
      }
    }).catch(error2);
    utils.waitForElement(`[data-id="MiniCartContent"] [data-id="SalePrice"], [data-id="MiniCartContent"] [data-id="SalePrice"]`).then(() => {
      window.sessionStorage.setItem("sale_item_in_cart", "true");
    }).catch(error2);
    utils.waitForElement(`[data-id="Interactive/EligibleSuccess"] h3[class^="w-success__title"], [data-id="CreateAccountSuccess"]`).then(() => {
      sendEvent(`pjs_new_account_created`);
      setAttributes({ account_sign_up: "true" });
      log("Attribute Assigned: New Account Created - Sign Up");
    }).catch(error2);
    utils.waitForElement(`button[data-id="MyAccountLink"]`).then((elAccountButton) => {
      elAccountButton.addEventListener("click", () => {
        if (window.innerWidth <= 1023) {
          sendEvent(`pjs_nav_header_icon_click_mobile`);
        } else {
          sendEvent(`pjs_nav_header_icon_click_desktop`);
        }
      });
    }).catch(error2);
    utils.waitForElement(`[data-id="EmailCampaignConfirmation"] [class^="confirmation__title"] span`).then((elConfirmationTitle) => {
      if (elConfirmationTitle.textContent && elConfirmationTitle.textContent.includes(`THANK YOU FOR SUBSCRIBING`)) {
        sendEvent(`pjs_email_signup_confirmation`);
      }
    }).catch(error2);
    onDataLayerPush((data) => {
      if (data?.originalUrl !== void 0) {
        if (data?.originalUrl.includes(".html")) {
          setAttributes({ pjs_landing_page_spp: "true" });
        } else {
          setAttributes({ pjs_landing_page_spp: "false" });
        }
      } else if (data.event === "e_idmIdAssignement") {
        const { visitorReturningCustomer } = data;
        setAttributes({ pjs_tb_api_returning_customer: visitorReturningCustomer ? "Purchaser" : "Non-purchaser" });
        if (data?.accountType !== void 0) {
          setAttributes({ pjs_tb_employee: data.accountType === "EMPLOYEE" });
        }
      } else if (data.event === "e_newsletter_subscribe") {
        sendEvent("pjs_newsletter_signup");
      }
    }, { dataLayerKey: "gtmDataLayer" });
  });
  onInitialized(() => {
    if (document.referrer.includes("google")) {
      setCookie("utm_source", "google");
    }
  });
}

// ../../account_tracking-broken.module.ts
account_tracking_broken_module_default({});

}();}catch(e){PJS.error(`account_tracking-broken.module`, e);}
account_tracking_WIP.module.js
try{
!function(){
// ../../_includes/oli/optimizely/attributes.ts
var setAttributes = (attributes, eventName = "push_attribute") => {
  window.optimizely.push({ type: "user", attributes });
  window.optimizely.push({ type: "event", eventName });
};
var setDefaultAttributes = (defaultAttributes, eventName = "push_attribute") => {
  const visitor = window.optimizely.get("visitor");
  const currentAttributes = Object.values(visitor.custom || {});
  const attributes = Object.entries(defaultAttributes).reduce((out, [key, value]) => {
    if (!currentAttributes.find((attr) => attr.id === key || attr.name === key))
      out[key] = value;
    return out;
  }, {});
  if (Object.keys(attributes).length) {
    setAttributes(attributes, eventName);
  }
};

// ../../_includes/cromedics/cookies.ts
function setCookie(name, value, {
  duration,
  domain = window.location.hostname.split(".").slice(-2).join("."),
  sameSite = "Lax"
} = {}) {
  let cookie = `${name}=${value}; Path=/`;
  let date;
  if (duration instanceof Date) {
    date = duration;
  } else if (duration > 0) {
    const hours = duration * 60 * 60 * 1e3;
    date = new Date();
    date.setTime(date.getTime() + hours);
  }
  if (date)
    cookie += `; Expires=${date.toUTCString()}`;
  if (domain)
    cookie += `; Domain=${domain}`;
  cookie += `; SameSite=${sameSite}`;
  if (sameSite.toLowerCase() === "none")
    cookie += `; Secure;`;
  document.cookie = cookie;
}

// ../../_includes/oli/optimizely/pages.ts
var onPageActivated = (callback, pageId) => {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "pageActivated" },
    handler: (event) => {
      if (!pageId || event.data.page.id === String(pageId))
        callback(event.data);
    }
  });
};
var onPageDeactivated = (callback, pageId) => {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "pageDeactivated" },
    handler: (event) => {
      if (!pageId || event.data.page.id === String(pageId))
        callback(event.data);
    }
  });
};
var pageTreatment = (callback, pageId) => {
  let undo;
  onPageActivated(() => {
    undo = callback();
  }, pageId);
  onPageDeactivated(() => {
    if (undo)
      undo();
  }, pageId);
};

// ../../_includes/oli/optimizely/lifecycle.ts
function onInitialized(callback) {
  window.optimizely.initialized && callback() || window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "initialized" },
    handler: callback
  });
}

// ../../_includes/oli/optimizely/events.ts
var sendEvent = (eventName, tags = {}) => {
  window.optimizely.push({
    type: "event",
    eventName,
    tags
  });
};

// @oli:logs:@oli:logs:account_tracking_WIP.module
var log = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[account_tracking_WIP.module]"))();
var error2 = /* @__PURE__ */ (() => window.CRO_PJS.error.bind(window, "account_tracking_WIP.module"))();

// ../../_includes/cromedics/monkey-patch.ts
function monkeyPatchBefore(object, key, callback) {
  const original = object[key];
  object[key] = function(...args) {
    try {
      callback(...args);
    } catch (e) {
      console.error(e);
    }
    return typeof original === "function" && original.apply(this, args);
  };
}

// ../../_includes/cromedics/datalayer.ts
var waitForDataLayer = (dataLayerKey = "dataLayer") => new Promise((resolve) => {
  (function poll() {
    if (!window[dataLayerKey] || typeof window[dataLayerKey].push !== "function")
      return setTimeout(poll, 50);
    return resolve(window[dataLayerKey]);
  })();
});
var onDataLayerPush = (callback, {
  dataLayerKey = "dataLayer",
  withPastData = true
} = {}) => waitForDataLayer(dataLayerKey).then((dataLayer) => {
  if (withPastData) {
    dataLayer.forEach((data) => {
      callback(data);
    });
  }
  monkeyPatchBefore(dataLayer, "push", callback);
});

// _projectjs/account_tracking_WIP.module.ts
var modalSignInBtn = '[data-id="AuthPortal"] [data-id="SignInForm"] button[data-id="SignInFormSubmitButton"]';
var miniCartSalePrice = '[data-id="MiniCart"] [data-id="SalePrice"], [data-id="MiniCartContent"] [data-id="SalePrice"]';
var accountSignUpMessage = '[data-id="Interactive/EligibleSuccess"] h3[class^="w-success__title"], [data-id="CreateAccountSuccess"]';
var navAccountText = '[data-id="MyAccountLink"] div[class^="element__text"]';
var emailSignUpConfirm = '[data-id="EmailCampaignConfirmation"] [class^="confirmation__title"] span';
var onModelVideoGrid = '[data-id="ProductTile-Slide-Active"] video, [data-id="ProductGallery-Slide-Active"] video';
var utilsDependentEvents = () => {
  const { waitForElement, waitUntil } = PJS.utils;
  const modalSignInBtnPromise = waitForElement(modalSignInBtn);
  const navAccountTextPromise = waitForElement(navAccountText);
  navAccountTextPromise.then((navAccountTextEl) => {
    navAccountTextEl.parentElement?.addEventListener("click", () => {
      if (window.innerWidth <= 1023)
        return sendEvent(`pjs_nav_header_icon_click_mobile`);
      sendEvent(`pjs_nav_header_icon_click_desktop`);
    });
  }).catch(error2);
  Promise.all([modalSignInBtnPromise, navAccountTextPromise]).then(([modalSignInBtnEl, navAccountTextEl]) => {
    modalSignInBtnEl.addEventListener("click", () => {
      waitUntil(() => navAccountTextEl.textContent !== "Sign in").then(() => {
        sendEvent(`pjs_sign_in_completed`);
      }).catch(error2);
    }, { once: true });
  }).catch(error2);
  waitForElement(miniCartSalePrice).then(() => {
    window.sessionStorage.setItem("sale_item_in_cart", "true");
  }).catch(error2);
  waitForElement(accountSignUpMessage).then(() => {
    sendEvent(`pjs_new_account_created`);
    setAttributes({ account_sign_up: "true" });
  }).catch(error2);
  waitForElement(emailSignUpConfirm).then((emailConfirmEl) => {
    if (emailConfirmEl.textContent?.includes(`THANK YOU FOR SUBSCRIBING`)) {
      sendEvent(`pjs_email_signup_confirmation`);
    }
  }).catch(error2);
};
var setUrlCookiesAndAttributes = () => {
  const { search, pathname } = window.location;
  if (pathname.includes("gated-springevent")) {
    setAttributes({ pjs_viewed_spring_event: "true" });
    setCookie(`pjs_viewed_spring_event`, `true`);
  }
  const params = new URLSearchParams(search);
  if (params.size === 0)
    return;
  for (const [paramName, paramValue] of params.entries()) {
    if (paramName.match(/utm_|gclsrc|journey/i)) {
      setCookie(paramName.toLowerCase(), paramValue.toLowerCase());
    }
  }
};
var dataLayerEvents = ({ originalUrl, event, accountType, visitorReturningCustomer }) => {
  if (originalUrl) {
    setAttributes({ pjs_landing_page_spp: originalUrl?.includes(".html") ? "true" : "false" });
  }
  if (event === "e_idmIdAssignement") {
    if (accountType) {
      setAttributes({ pjs_tb_employee: accountType === "EMPLOYEE" });
    }
    return setAttributes({ pjs_tb_api_returning_customer: visitorReturningCustomer ? "Purchaser" : "Non-purchaser" });
  }
  if (event === "e_newsletter_subscribe") {
    sendEvent("pjs_newsletter_signup");
  }
};
function teardown(cancelFuncs) {
  let thisFunc;
  while (thisFunc = cancelFuncs.pop()) {
    if (thisFunc)
      thisFunc();
  }
}
function account_tracking_WIP_module_default() {
  onDataLayerPush((data) => dataLayerEvents(data), { dataLayerKey: "gtmDataLayer" });
  onInitialized(() => {
    setDefaultAttributes({ pjs_tb_api_returning_customer: "Unknown" });
    if (document.referrer.includes("google")) {
      setCookie("utm_source", "google");
    }
    utilsDependentEvents();
    setUrlCookiesAndAttributes();
  });
  log(`add page treatment`);
  pageTreatment(() => {
    const cancelFuncs = [];
    const { observeSelector, waitUntil, waitForElement } = PJS.utils;
    cancelFuncs.push(observeSelector(onModelVideoGrid, () => {
      setAttributes({ pjs_user_saw_on_model_video: "true" }, "pjs_user_saw_on_model_video");
    }));
    cancelFuncs.push(observeSelector('[data-id="ProductTile"]', (thisProduct) => {
      const { productId } = thisProduct.dataset;
      const pdpVisited = waitUntil(() => window.location.pathname.includes(`${productId}.html`));
      pdpVisited.then(() => {
        waitForElement(`[data-id="ProductGallery-Slide-Active"] video`).then(() => {
          setAttributes({ pjs_user_saw_on_model_video: "true" }, "pjs_user_saw_on_model_video");
        }).catch(error2);
      }).catch(error2);
    }));
    return () => {
      teardown(cancelFuncs);
    };
  }, 25343490080);
}

// ../../account_tracking_WIP.module.ts
account_tracking_WIP_module_default({});

}();}catch(e){PJS.error(`account_tracking_WIP.module`, e);}
add-to-cart-tracking-WIP.module.js
try{
!function(){
// ../../_includes/cromedics/monkey-patch.ts
function monkeyPatchBefore(object, key, callback) {
  const original = object[key];
  object[key] = function(...args) {
    try {
      callback(...args);
    } catch (e) {
      console.error(e);
    }
    return typeof original === "function" && original.apply(this, args);
  };
}

// ../../_includes/cromedics/datalayer.ts
var waitForDataLayer = (dataLayerKey = "dataLayer") => new Promise((resolve) => {
  (function poll() {
    if (!window[dataLayerKey] || typeof window[dataLayerKey].push !== "function")
      return setTimeout(poll, 50);
    return resolve(window[dataLayerKey]);
  })();
});
var onDataLayerPush = (callback, {
  dataLayerKey = "dataLayer",
  withPastData = true
} = {}) => waitForDataLayer(dataLayerKey).then((dataLayer) => {
  if (withPastData) {
    dataLayer.forEach((data) => {
      callback(data);
    });
  }
  monkeyPatchBefore(dataLayer, "push", callback);
});

// ../../_includes/oli/optimizely/lifecycle.ts
function onActivated(callback) {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "activated" },
    handler: callback
  });
}

// @oli:logs:@oli:logs:add-to-cart-tracking-WIP.module
var log = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[add-to-cart-tracking-WIP.module]"))();

// ../../_includes/oli/optimizely/attributes.ts
var setAttributes = (attributes, eventName = "push_attribute") => {
  window.optimizely.push({ type: "user", attributes });
  window.optimizely.push({ type: "event", eventName });
};

// _includes/productAttribution.ts
var families = {
  tMonogram: ["t-monogram", "t monogram", "tmonogram"],
  fleming: ["fleming"],
  ella: ["ella"],
  eleanor: ["eleanor"],
  leeRadziwill: ["lee radziwill", "lee-radziwill"],
  robinson: ["robinson"],
  kira: ["kira"],
  miller: ["miller"],
  mcgraw: ["mcgraw"],
  bonBon: ["bon bon", "bon-bon"],
  claire: ["claire"],
  minnie: ["minnie"],
  goodLuckTrainer: ["good luck trainer", "good-luck-trainer"],
  ladybugSneaker: ["ladybug sneaker", "ladybug-sneaker"],
  court: ["court"]
};
var categories = {
  sale: ["sale"],
  shoes: ["shoes", "shoe"],
  handbags: ["handbags", "handbag", "hbg"]
};
var subcategories = {
  sneakers: ["sneakers", "sneaker"],
  sandals: ["sandals", "sandal"],
  boots: ["boots", "boot"],
  balletFlats: ["ballet flats", "ballet-flats", "ballet", "flat", "flats", "ballet flat", "ballet-flat", "ballets", "ballets flat", "ballets-flat", "ballets flats"],
  loafers: ["loafers", "loafer"],
  totes: ["totes", "tote"],
  shoulderCrossbodyBags: ["shoulder bags", "shoulder bag", "crossbody bags", "crossbody bag"],
  bucketBags: ["bucket bags", "bucket bag"]
};
var getFamilyAttributeName = (product) => {
  const productName = product.productName?.toLowerCase();
  if (productName) {
    for (const [familyName, familyTags] of Object.entries(families)) {
      for (const tag of familyTags) {
        if (productName.includes(tag))
          return familyName;
      }
    }
  }
  return void 0;
};
var getCategoryAttributeName = (product) => {
  const productName = product.productName?.toLowerCase();
  if (productName) {
    for (const [categoryName, categoryTags] of Object.entries(categories)) {
      for (const tag of categoryTags) {
        if (productName.includes(tag))
          return categoryName;
      }
    }
  }
  return void 0;
};
var getSubcategoryAttributeName = (product) => {
  const productName = product.productName?.toLowerCase();
  if (productName) {
    for (const [subcategoryName, subcategoryTags] of Object.entries(subcategories)) {
      for (const tag of subcategoryTags) {
        if (productName.includes(tag))
          return subcategoryName;
      }
    }
  }
  return void 0;
};

// _projectjs/add-to-cart-tracking-WIP.module.ts
function add_to_cart_tracking_WIP_module_default() {
  const cache = {
    addedProducts: {},
    watchlist: {},
    tracking: []
    // list of tags
  };
  const getStoredProducts = (tag) => {
    return JSON.parse(sessionStorage.getItem(`cro_${tag}_items`)) || [];
  };
  const addStoredProduct = (tag, productSKU) => {
    const products = cache.addedProducts[tag] || [];
    if (!products.includes(productSKU)) {
      products.push(productSKU);
      sessionStorage.setItem(`cro_${tag}_items`, JSON.stringify(products));
      log(`Added product [${productSKU}] to "cro_${tag}_items" session storage.`);
    } else {
      log(`Product [${productSKU}] is already added to "cro_${tag}_items" session storage.`);
    }
  };
  onDataLayerPush((data) => {
    if (data.event !== "e_addToCart" || !data.addedProduct)
      return;
    log(`ATC product data:`, data.addedProduct);
    setAttributes({ pjs_user_atc: "true" });
    const { sku, id, productName, listPrice, price } = data.addedProduct;
    const trackProduct = (tag) => {
      setAttributes({
        [`pjs_${tag}_atc`]: "true"
      }, `pjs_${tag}_atc`);
      addStoredProduct(tag, sku);
    };
    cache.tracking.forEach(trackProduct);
    const taxonomyArray = ["taxonomyL1", "taxonomyL2", "taxonomyL3"];
    taxonomyArray.forEach((thisTaxonomy) => {
      if (data.addedProduct[`${thisTaxonomy}`] !== "") {
        trackProduct(data.addedProduct[`${thisTaxonomy}`]);
        if (price < listPrice)
          trackProduct(`sale_${data.addedProduct[`${thisTaxonomy}`]}`);
      }
    });
    if (price < listPrice)
      trackProduct(`sale_product`);
    if (productName.toLowerCase().includes("tote") && data.addedProduct.taxonomyL2 !== "handbags-tote-bag") {
      trackProduct(`handbags-tote-bag`);
    }
    const familyProduct = getFamilyAttributeName(data.addedProduct);
    const category = getCategoryAttributeName(data.addedProduct);
    const subcategory = getSubcategoryAttributeName(data.addedProduct);
    if (familyProduct)
      setAttributes({ ["User Add To Cart"]: familyProduct });
    if (category)
      setAttributes({ ["User Add To Cart"]: category });
    if (subcategory)
      setAttributes({ ["User Add To Cart"]: subcategory });
    if (sessionStorage.getItem(`productWatchlist`)) {
      const watch = JSON.parse(sessionStorage.getItem(`productWatchlist`) || "");
      for (const tag in watch) {
        if (watch[tag].some((watchedId) => watchedId === id)) {
          trackProduct(tag);
          const updateArray = watch[tag].splice(watch[tag].indexOf(id) + 1, 1);
          watch[tag] = updateArray;
          sessionStorage.setItem(`productWatchlist`, JSON.stringify(watch));
        }
      }
    }
  }, { dataLayerKey: "gtmDataLayer" });
  onActivated(() => {
    log(`Resetting add to cart tracking.`);
    cache.tracking = [];
  });
  PJS.utils.trackPageATC = (tag, watchlist) => {
    setTimeout(() => {
      log(`Tracking "${tag}" Add To Cart metrics.`);
      if (Array.isArray(watchlist)) {
        cache.watchlist[tag] = watchlist;
      } else {
        cache.watchlist[tag] = [];
      }
      sessionStorage.setItem(`productWatchlist`, JSON.stringify(cache.watchlist));
      if (cache.tracking.includes(tag))
        return;
      cache.addedProducts[tag] = getStoredProducts(tag);
      cache.tracking.push(tag);
    });
  };
  PJS.utils.trackProduct = (tag, watchlist) => {
    setTimeout(() => {
      log(`Tracking "${tag}" Add To Cart metrics.`);
      cache.watchlist[tag] = [];
      if (Array.isArray(watchlist)) {
        watchlist.forEach((thisProduct) => {
          cache.watchlist[tag].push(thisProduct);
        });
        sessionStorage.setItem(`productWatchlist`, JSON.stringify(cache.watchlist));
      }
    });
  };
}

// ../../add-to-cart-tracking-WIP.module.ts
add_to_cart_tracking_WIP_module_default({});

}();}catch(e){PJS.error(`add-to-cart-tracking-WIP.module`, e);}
add-to-cart-tracking.module.js
try{
!function(){
// ../../_includes/cromedics/monkey-patch.ts
function monkeyPatchBefore(object, key, callback) {
  const original = object[key];
  object[key] = function(...args) {
    try {
      callback(...args);
    } catch (e) {
      console.error(e);
    }
    return typeof original === "function" && original.apply(this, args);
  };
}

// ../../_includes/cromedics/datalayer.ts
var waitForDataLayer = (dataLayerKey = "dataLayer") => new Promise((resolve) => {
  (function poll() {
    if (!window[dataLayerKey] || typeof window[dataLayerKey].push !== "function")
      return setTimeout(poll, 50);
    return resolve(window[dataLayerKey]);
  })();
});
var onDataLayerPush = (callback, {
  dataLayerKey = "dataLayer",
  withPastData = true
} = {}) => waitForDataLayer(dataLayerKey).then((dataLayer) => {
  if (withPastData) {
    dataLayer.forEach((data) => {
      callback(data);
    });
  }
  monkeyPatchBefore(dataLayer, "push", callback);
});

// ../../_includes/oli/optimizely/lifecycle.ts
function onActivated(callback) {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "activated" },
    handler: callback
  });
}

// @oli:logs:@oli:logs:add-to-cart-tracking.module
var log = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[add-to-cart-tracking.module]"))();

// ../../_includes/oli/optimizely/attributes.ts
var setAttributes = (attributes, eventName = "push_attribute") => {
  window.optimizely.push({ type: "user", attributes });
  window.optimizely.push({ type: "event", eventName });
};

// _projectjs/add-to-cart-tracking.module.ts
function add_to_cart_tracking_module_default() {
  const cache = {
    addedProducts: {},
    watchlist: {},
    tracking: []
    // list of tags
  };
  const getStoredProducts = (tag) => {
    return JSON.parse(sessionStorage.getItem(`cro_${tag}_items`)) || [];
  };
  const addStoredProduct = (tag, productSKU) => {
    const products = cache.addedProducts[tag] || [];
    if (!products.includes(productSKU)) {
      products.push(productSKU);
      sessionStorage.setItem(`cro_${tag}_items`, JSON.stringify(products));
      log(`Added product [${productSKU}] to "cro_${tag}_items" session storage.`);
    } else {
      log(`Product [${productSKU}] is already added to "cro_${tag}_items" session storage.`);
    }
  };
  onDataLayerPush((data) => {
    if (data.event !== "e_addToCart" || !data.addedProduct)
      return;
    log(`ATC product data:`, data.addedProduct);
    setAttributes({ pjs_user_atc: "true" });
    const { sku, id, productName, listPrice, price } = data.addedProduct;
    const trackProduct = (tag) => {
      setAttributes({
        [`pjs_${tag}_atc`]: "true"
      }, `pjs_${tag}_atc`);
      addStoredProduct(tag, sku);
    };
    cache.tracking.forEach(trackProduct);
    const taxonomyArray = ["taxonomyL1", "taxonomyL2", "taxonomyL3"];
    taxonomyArray.forEach((thisTaxonomy) => {
      if (data.addedProduct[`${thisTaxonomy}`] !== "") {
        trackProduct(data.addedProduct[`${thisTaxonomy}`]);
        if (price < listPrice)
          trackProduct(`sale_${data.addedProduct[`${thisTaxonomy}`]}`);
      }
    });
    if (price < listPrice)
      trackProduct(`sale_product`);
    if (productName.toLowerCase().includes("tote") && data.addedProduct.taxonomyL2 !== "handbags-tote-bag") {
      trackProduct(`handbags-tote-bag`);
    }
    if (sessionStorage.getItem(`productWatchlist`)) {
      const watch = JSON.parse(sessionStorage.getItem(`productWatchlist`) || "");
      for (const tag in watch) {
        if (watch[tag].some((watchedId) => watchedId === id)) {
          trackProduct(tag);
          const updateArray = watch[tag].splice(watch[tag].indexOf(id) + 1, 1);
          watch[tag] = updateArray;
          sessionStorage.setItem(`productWatchlist`, JSON.stringify(watch));
        }
      }
    }
  }, { dataLayerKey: "gtmDataLayer" });
  onActivated(() => {
    log(`Resetting add to cart tracking.`);
    cache.tracking = [];
  });
  PJS.utils.trackPageATC = (tag, watchlist) => {
    setTimeout(() => {
      log(`Tracking "${tag}" Add To Cart metrics.`);
      if (Array.isArray(watchlist)) {
        cache.watchlist[tag] = watchlist;
      } else {
        cache.watchlist[tag] = [];
      }
      sessionStorage.setItem(`productWatchlist`, JSON.stringify(cache.watchlist));
      if (cache.tracking.includes(tag))
        return;
      cache.addedProducts[tag] = getStoredProducts(tag);
      cache.tracking.push(tag);
    });
  };
  PJS.utils.trackProduct = (tag, watchlist) => {
    setTimeout(() => {
      log(`Tracking "${tag}" Add To Cart metrics.`);
      cache.watchlist[tag] = [];
      if (Array.isArray(watchlist)) {
        watchlist.forEach((thisProduct) => {
          cache.watchlist[tag].push(thisProduct);
        });
        sessionStorage.setItem(`productWatchlist`, JSON.stringify(cache.watchlist));
      }
    });
  };
}

// ../../add-to-cart-tracking.module.ts
add_to_cart_tracking_module_default({});

}();}catch(e){PJS.error(`add-to-cart-tracking.module`, e);}
general--cro-metrics-utilities.Optimizely.module.js
try{
!function(){
// ../../_includes/cromedics/cookies.ts
function setCookie(name, value, {
  duration,
  domain = window.location.hostname.split(".").slice(-2).join("."),
  sameSite = "Lax"
} = {}) {
  let cookie = `${name}=${value}; Path=/`;
  let date;
  if (duration instanceof Date) {
    date = duration;
  } else if (duration > 0) {
    const hours = duration * 60 * 60 * 1e3;
    date = new Date();
    date.setTime(date.getTime() + hours);
  }
  if (date)
    cookie += `; Expires=${date.toUTCString()}`;
  if (domain)
    cookie += `; Domain=${domain}`;
  cookie += `; SameSite=${sameSite}`;
  if (sameSite.toLowerCase() === "none")
    cookie += `; Secure;`;
  document.cookie = cookie;
}
function getCookie(name) {
  const nameEQ = `${name}=`;
  const cookieArray = document.cookie.split(";");
  for (const cookie of cookieArray) {
    const cookieName = cookie.trim();
    if (cookieName.indexOf(nameEQ) === 0)
      return cookieName.substring(nameEQ.length, cookieName.length);
  }
  return null;
}
function delCookie(name, {
  domain = window.location.hostname.split(".").slice(-2).join(".")
} = {}) {
  let cookie = `${name}=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/`;
  if (domain)
    cookie += `; domain=${domain}`;
  document.cookie = cookie;
}

// ../../_includes/oli/optimizely/lifecycle.ts
function onInitialized(callback) {
  window.optimizely.initialized && callback() || window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "initialized" },
    handler: callback
  });
}
function onTrackEvent(callback) {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "analytics", name: "trackEvent" },
    handler: (event) => {
      event.data.id = event.data.id || event.data.apiName;
      callback(event.data);
    }
  });
}

// ../../_includes/cromedics/params.ts
var getParam = (param, search = window.location.search) => new URLSearchParams(search).get(param);

// ../../_includes/cromedics/cro-mode.ts
var PARAM_NAME = "cro_mode";
var defaultOptions = {
  duration: window.CRO_PJS?.COOKIE_DURATION || 1,
  // Cro Mode lasts for 1 hour by default
  domain: window.CRO_PJS?.COOKIE_DOMAIN
};
var setCroMode = (newMode, addOptions = {}) => {
  const options = Object.assign({}, defaultOptions, addOptions);
  if (newMode && /^[\w\-_]+$/.test(newMode)) {
    setCookie(PARAM_NAME, newMode, options);
    console.log(`Cro Metrics "${newMode}" mode enabled.`);
    return newMode;
  }
  delCookie(PARAM_NAME, options);
  console.log(`Cro Metrics logging and qa modes disabled.`);
  return void 0;
};
var getCroMode = () => getParam(PARAM_NAME) || getCookie(PARAM_NAME) || void 0;

// ../../_includes/cromedics/logging.ts
var PJS_FORMAT = "color:white;background:#12659d;";
var EXPERIMENT_FORMAT = "color:white;background:#ff590b;";
var NOOP = function(..._args) {
};
function getLogFn(shouldLog, prefix, fn = "info") {
  return shouldLog ? console[fn].bind(console, `%c[${prefix}]`, prefix === "PJS" ? PJS_FORMAT : EXPERIMENT_FORMAT) : NOOP;
}
function experimentError(errorLocation, details) {
  console.error("%c[cro]", EXPERIMENT_FORMAT, `[${errorLocation}]`, details);
}
function pjsError(errorLocation, details) {
  console.error("%c[PJS]", PJS_FORMAT, `[${errorLocation}]`, details);
}
var pjsAssert = (assertion, tagOrName, errorMsg, value) => {
  console.assert(assertion, "%0", { value, errorMessage: `[PJS] ${tagOrName}: ${errorMsg}` });
};
var experimentAssert = (assertion, tagOrName, errorMsg, value) => {
  console.assert(assertion, "%0", { value, errorMessage: `[cro] ${tagOrName}: ${errorMsg}` });
};

// ../../_includes/pjs/utilities.ts
function initUtilities(PJS2) {
  const utils = PJS2.utils || (PJS2.utils = {});
  utils.cookie = {
    set: setCookie,
    get: getCookie,
    del: delCookie
  };
  utils.getParam = getParam;
  PJS2.log = utils.log = PJS2.debug = utils.debug = PJS2.assert = utils.assert = NOOP;
  utils.error = experimentError;
  PJS2.error = pjsError;
  PJS2.setMode = (newMode, options) => {
    PJS2.mode = setCroMode(newMode, options);
    const shouldLog = PJS2.mode !== void 0;
    PJS2.log = getLogFn(shouldLog, "PJS");
    PJS2.debug = getLogFn(shouldLog, "PJS", "debug");
    PJS2.assert = shouldLog ? pjsAssert : NOOP;
    PJS2.utils.log = getLogFn(shouldLog, "cro");
    PJS2.utils.assert = shouldLog ? experimentAssert : NOOP;
    PJS2.utils.debug = getLogFn(shouldLog, "cro", "debug");
  };
  const initialMode = getCroMode() || PJS2.mode;
  if (initialMode)
    PJS2.setMode(initialMode);
}

// ../../_projectjs/general/cro-metrics-utilities.Optimizely.module.ts
function cro_metrics_utilities_Optimizely_module_default() {
  initUtilities(PJS);
  onInitialized(() => {
    const utils = window.optimizely.get("utils");
    Object.assign(utils, PJS.utils);
    PJS.utils.observeSelector = utils.observeSelector;
    PJS.utils.waitForElement = utils.waitForElement;
    PJS.utils.waitUntil = utils.waitUntil;
  });
  onTrackEvent((data) => {
    PJS.log(`Metric fired: ${data.name} <${data.apiName}>`, data.tags);
  });
}

// ../../general__cro-metrics-utilities.Optimizely.module.ts
cro_metrics_utilities_Optimizely_module_default({});

}();}catch(e){PJS.error(`general/cro-metrics-utilities.Optimizely.module`, e);}
general--detect-async.module.js
try{
!function(){
// ../../_projectjs/general/detect-async.module.ts
function detect_async_module_default() {
  if (document.body) {
    PJS.log("Nonstandard snippet loading detected! See https://support.crometrics.com/engineering/why-to-not-use-async-snippet-installations for details.");
  }
}

// ../../general__detect-async.module.ts
detect_async_module_default({});

}();}catch(e){PJS.error(`general/detect-async.module`, e);}
history-state-change.module.js
try{
!function(){
// ../../_includes/oli/optimizely/lifecycle.ts
function activate() {
  window.optimizely.push({ type: "activate" });
}

// ../../_includes/cromedics/monkey-patch.ts
function monkeyPatchAfter(object, key, callback) {
  const original = object[key];
  object[key] = function(...args) {
    const output = typeof original === "function" && original.apply(this, args);
    try {
      callback(...args);
    } catch (e) {
      console.error(e);
    }
    return output;
  };
}

// _projectjs/history-state-change.module.ts
var convertURL = (url) => {
  const converter = document.createElement("a");
  converter.href = url;
  return converter.href;
};
var lastUrl = window.location.href;
var urlChanged = (newUrl) => {
  if (newUrl === lastUrl)
    return;
  lastUrl = newUrl;
  window.dispatchEvent(new CustomEvent("spaPageEnd"));
  setTimeout(activate);
};
var stateChangeCallback = (state, title, url) => {
  urlChanged(convertURL(url));
};
function history_state_change_module_default() {
  monkeyPatchAfter(window.history, "pushState", stateChangeCallback);
  monkeyPatchAfter(window.history, "replaceState", stateChangeCallback);
  window.addEventListener("popstate", () => {
    urlChanged(window.location.href);
  });
}

// ../../history-state-change.module.ts
history_state_change_module_default({});

}();}catch(e){PJS.error(`history-state-change.module`, e);}
individual_add_to_cart.module.js
try{
!function(){
// @oli:logs:@oli:logs:individual_add_to_cart.module
var log = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[individual_add_to_cart.module]"))();

// ../../_includes/oli/optimizely/attributes.ts
var setAttributes = (attributes, eventName = "push_attribute") => {
  window.optimizely.push({ type: "user", attributes });
  window.optimizely.push({ type: "event", eventName });
};

// ../../_includes/oli/optimizely/pages.ts
var onPageActivated = (callback, pageId) => {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "pageActivated" },
    handler: (event) => {
      if (!pageId || event.data.page.id === String(pageId))
        callback(event.data);
    }
  });
};
var onPageDeactivated = (callback, pageId) => {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "pageDeactivated" },
    handler: (event) => {
      if (!pageId || event.data.page.id === String(pageId))
        callback(event.data);
    }
  });
};
var pageTreatment = (callback, pageId) => {
  let undo;
  onPageActivated(() => {
    undo = callback();
  }, pageId);
  onPageDeactivated(() => {
    if (undo)
      undo();
  }, pageId);
};

// @oli:logs:@oli:logs:trackProduct
var log2 = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[trackProduct]"))();

// _includes/trackProduct.ts
var cache = {
  addedProducts: {},
  watchlist: {},
  tracking: []
  // list of tags
};
var trackProduct = (tag, watchlist) => {
  setTimeout(() => {
    log2(`Tracking "${tag}" Add To Cart metrics.`);
    cache.watchlist[tag] = [];
    if (Array.isArray(watchlist)) {
      watchlist.forEach((product) => {
        cache.watchlist[tag].push(product);
      });
      sessionStorage.setItem(
        `productWatchlist`,
        JSON.stringify(cache.watchlist)
      );
    }
  });
};

// _projectjs/individual_add_to_cart.module.ts
function individual_add_to_cart_module_default() {
  const eventMap = [
    // atb from or after clicking product recently viewed or ymal carousels on cart page
    {
      pageName: "cartPage",
      pageId: 17037790526,
      metrics: [
        {
          atc: `cart_ymal_recentlyviewed`,
          engagement: `pjs_recently_viewed_engagement_cart`,
          productSelector: `[data-carousel-type="recently-viewed"] [data-id="ProductTile"]`
        },
        {
          atc: `cart_ymal`,
          productSelector: `[data-product-list="recommended"] [data-id="ProductTile"]`
        },
        {
          atc: `cross_sell_cart`,
          productSelector: `[data-product-list="recommended"] [data-id="ProductTile"]`,
          engagement: `pjs_cross_sell_engagement_cart`
        }
      ]
    },
    {
      pageName: "pdpPage",
      pageId: 15011040160,
      metrics: [
        {
          atc: `pdp_ymal`,
          engagement: `pdp_ymal_engagement`,
          productSelector: `[data-product-list="recommended"] [data-id="ProductTile"]`
        },
        {
          atc: `pdp_recently_viewed`,
          engagement: `pjs_pdp_recently_viewed_engagement`,
          productSelector: `[data-carousel-type="recently-viewed"] [data-id="ProductTile"]`
        },
        {
          atc: `pdp_styled_with`,
          engagement: `pjs_pdp_styled_with_engagement`,
          productSelector: `[data-carousel-type="styled-with"] [data-id="ProductTile"]`
        },
        {
          atc: `pdp_all_caro`,
          engagement: `pjs_pdp_all_engagement`,
          productSelector: `[data-id="CrossSell"] [data-id="ProductTile"]`
        },
        {
          atc: `pdp_tabbed_caro`,
          engagement: `pdp_tabbed_caro_engagement`,
          productSelector: `[data-id="SppEditorialContent-purchase-tabbed-carousel---all-spps"] [data-id="ProductTile"]`
        }
      ]
    },
    {
      pageName: "gridPage",
      pageId: 14942591331,
      metrics: [
        {
          atc: `grid_page`,
          productSelector: `[data-id="ProductTile"]`
        }
      ]
    },
    {
      pageName: "readyToWear",
      pageId: 25343490080,
      metrics: [
        {
          atc: `ready_to_wear`,
          productSelector: `[data-id="ProductTile"]`
        }
      ]
    }
  ];
  function teardown(cancelFuncs) {
    let thisFunc;
    while (thisFunc = cancelFuncs.pop()) {
      if (thisFunc)
        thisFunc();
    }
  }
  for (const page in eventMap) {
    pageTreatment(() => {
      const cancelFuncs = [];
      const { observeSelector } = PJS.utils;
      eventMap[page].metrics.forEach((thisMetric) => {
        const { engagement, atc, productSelector } = thisMetric;
        const storage = sessionStorage.getItem(`cro_${atc}_items`);
        const itemAdded = storage ? JSON.parse(storage) : [];
        cancelFuncs.push(observeSelector(productSelector, (products) => {
          const { productId } = products.dataset;
          itemAdded.push(productId);
          const itemsToStore = JSON.stringify(itemAdded);
          sessionStorage.setItem(`cro_${atc}_items`, itemsToStore);
          products.addEventListener("click", (e) => {
            if (productId && atc) {
              log(`Track ${productId}`);
              trackProduct(atc, [productId]);
            }
            if (engagement) {
              setAttributes({ [engagement]: "true" }, engagement);
              if (!e.target)
                return;
              const clickedEl = e.target;
              if (clickedEl.localName === "figure" || clickedEl.localName === "a") {
                setAttributes({ [`${engagement}_pdp_viewed`]: "true" }, `${engagement}_pdp_viewed`);
              }
            }
          });
        }));
      });
      return () => {
        teardown(cancelFuncs);
      };
    }, eventMap[page].pageId);
  }
}

// ../../individual_add_to_cart.module.ts
individual_add_to_cart_module_default({});

}();}catch(e){PJS.error(`individual_add_to_cart.module`, e);}
initFeatureFlag.module.js
try{
!function(){
// ../../_includes/cromedics/cookies.ts
function getCookie(name) {
  const nameEQ = `${name}=`;
  const cookieArray = document.cookie.split(";");
  for (const cookie of cookieArray) {
    const cookieName = cookie.trim();
    if (cookieName.indexOf(nameEQ) === 0)
      return cookieName.substring(nameEQ.length, cookieName.length);
  }
  return null;
}

// ../../_includes/oli/optimizely/lifecycle.ts
function onInitialized(callback) {
  window.optimizely.initialized && callback() || window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "initialized" },
    handler: callback
  });
}

// @oli:logs:@oli:logs:initFeatureFlag.module
var log = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[initFeatureFlag.module]"))();
var error2 = /* @__PURE__ */ (() => window.CRO_PJS.error.bind(window, "initFeatureFlag.module"))();

// @oli:logs:@oli:logs:FeatureFlag
var log2 = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[FeatureFlag]"))();

// @oli:logs:@oli:logs:FeatureBase
var log3 = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[FeatureBase]"))();

// _includes/FeatureBase.ts
var validDomains = ["www.toryburch.com", "toryburch.com", "com.qa2.aem.toryburch.com", "com.stg.aem.toryburch.com", "com.uat.aem.toryburch.com", "na.preview.prod.aem.toryburch.com"];
var FeatureBase = class {
  constructor(experimentId, tag = `Experiment ID:${experimentId}`, keyPrefix = "bucketing") {
    if (!validDomains.includes(window.location.hostname))
      throw new Error("This can only be run on select domains.");
    this.keyPrefix = keyPrefix;
    this.experimentId = experimentId;
    this.experimentData = window.optimizely.get("data").experiments[this.experimentId];
    this.numVariations = this.experimentData.variations ? this.experimentData.variations.length : 2;
    this.tag = tag;
    const variationIndex = this.variationIndex = this.getVariationIndex();
    this.setStoredVariationIndex(variationIndex);
  }
  /**
   * The key used for localStorage
   */
  get localStorageKey() {
    return `cro_${this.keyPrefix}_${this.experimentData?.id}`;
  }
  getStoredVariationIndex() {
    const index = window.localStorage.getItem(this.localStorageKey);
    return index !== null ? parseInt(index) : null;
  }
  setStoredVariationIndex(variationIndex) {
    window.localStorage.setItem(this.localStorageKey, String(variationIndex));
  }
  getOptimizelyForcedIndex() {
    const forceParameter = new URLSearchParams(window.location.search).get("optimizely_x");
    if (forceParameter !== null) {
      const forceVariation = this.experimentData.variations.findIndex((o) => o.id === forceParameter.split(",")[0]);
      return forceVariation > -1 ? forceVariation : 0;
    }
    return null;
  }
  /**
   * Gets the variation index that should be displayed to the visitor (0 = v0, 1 = v1, etc).
   * In cro_mode=qa, if there is a force variation link, we force the variation. If invalid, return control.
   * Leverages both the force query params (used by the client) and our localStorage key used for persistence.
   * If no index is provided by these methods a random variation index will be returned.
   */
  getVariationIndex() {
    const forcedIndex = this.getOptimizelyForcedIndex();
    if (forcedIndex !== null)
      return forcedIndex;
    const storedIndex = this.getStoredVariationIndex();
    if (storedIndex !== null)
      return storedIndex;
    return Math.floor(Math.random() * this.numVariations);
  }
  /**
   * @param {number?} forceVariation [optional] index of the variation you want to force. 0 = v0, 1 = v1, etc
   */
  bucketVisitor() {
    log3(`Bucketing ${this.tag} into v${this.variationIndex}.`);
    window.optimizely.push({
      type: "bucketVisitor",
      experimentId: `${this.experimentId}`,
      variationId: `${this.experimentData.variations[this.variationIndex]?.id}`
    });
  }
};

// _includes/FeatureFlag.ts
if (!window.__AB_FEATURES__)
  window.__AB_FEATURES__ = {};
var FeatureFlag = class extends FeatureBase {
  constructor(featureConfig, experimentId, tag) {
    super(experimentId, tag, "feature");
    this.variationFeatures = [
      []
      // initalize v0 with no features
    ];
    if (!featureConfig)
      throw new Error("Invalid feature flags configuration.");
    let defaultVariationFeatures = [];
    if (typeof featureConfig === "string") {
      defaultVariationFeatures = [featureConfig];
    } else if (Array.isArray(featureConfig)) {
      if (!featureConfig.length)
        throw new Error("Invalid feature flags configuration.");
      defaultVariationFeatures = featureConfig;
    }
    for (let variationIndex = 1; variationIndex < this.numVariations; variationIndex++) {
      this.variationFeatures[variationIndex] = defaultVariationFeatures;
    }
    if (typeof featureConfig === "object" && !Array.isArray(featureConfig)) {
      const variationTags = Object.keys(featureConfig);
      for (const variationTag of variationTags) {
        const variationIndex = parseInt(variationTag.substring(1));
        if (!Number.isInteger(variationIndex) || variationIndex >= this.numVariations) {
          throw new Error(`Invalid feature flags configuration: "${variationTag}" variation is invalid.`);
        }
        const features = featureConfig[variationTag];
        this.variationFeatures[variationIndex] = typeof features === "string" ? [features] : features;
      }
    }
  }
  /**
   * The Tory Burch site allows for certain force query parameters to turn on these feature flags.
   * Feature flags turned on using this method overrides the use of `window.__AB_FEATURES__` and the
   * flag is automatically persisted in localStorage key `persistenceFeatures`.
   *
   * @example https://www.toryburch.com/en-us/clothing/dresses/?nfp=true essentially turns on the
   * `window.__AB_FEATURES__['nfp'] = true` flag, however it doesn't actually use the `window.__AB_FEATURES__` object.
   * The query param overrides whatever we set in `window.__AB_FEATURES__` so these settings may conflict with one another.
   * The localStorage that it sets (`persistenceFeatures`) also overrides whatever is in `window.__AB_FEATURES__`.
   */
  detectFeatureForceParam() {
    const urlParams = new URLSearchParams(window.location.search);
    for (const i in this.variationFeatures) {
      const variationIndex = parseInt(i);
      const features = this.variationFeatures[i];
      for (const f in features) {
        const param = urlParams.get(features[f]);
        if (param !== null) {
          return param === "true" ? variationIndex : 0;
        }
      }
    }
    return null;
  }
  getVariationIndex() {
    const forceParamIndex = this.detectFeatureForceParam();
    if (forceParamIndex !== null)
      return forceParamIndex;
    return super.getVariationIndex();
  }
  activate() {
    super.bucketVisitor();
    const features = this.variationFeatures[this.variationIndex];
    if (features.length) {
      log2(`[${this.tag}] Activating ${features.join(", ")} feature flag(s)!`);
      features.forEach((feature) => {
        if (feature.includes(",")) {
          feature.split(",").forEach((thisFeature) => {
            window.__AB_FEATURES__[thisFeature] = true;
          });
        } else {
          window.__AB_FEATURES__[feature] = true;
        }
      });
    }
  }
};

// _projectjs/initFeatureFlag.module.ts
var INTEGRATION_ID = "21808030171";
var parseFeatureFlags = (flags) => {
  const firstCharacter = flags.trimStart().substring(0, 1);
  if (firstCharacter === "[" || firstCharacter === "{")
    return JSON.parse(flags);
  return flags.split(",").map((item) => item.trim());
};
function initFeatureFlag_module_default() {
  onInitialized(() => {
    const { campaigns } = window.optimizely.get("data");
    for (const campaign of Object.values(campaigns)) {
      try {
        const audience = campaign.experiments?.[0].audienceName;
        if (audience.includes(`QA`) && !(window.location.search.includes(`cro_mode=qa`) || getCookie(`cro_mode`) === "qa")) {
          log(`Audience not matched: aborting`);
        } else {
          const experimentID = parseInt(campaign.experiments?.[0].id);
          const integrationData = campaign?.integrationSettings?.[INTEGRATION_ID];
          const features = integrationData?.features;
          const experimentTag = integrationData?.experimentTag;
          if (experimentID && features) {
            const flag = new FeatureFlag(parseFeatureFlags(features), experimentID, experimentTag);
            flag.activate();
          }
        }
      } catch (e) {
        error2(e);
      }
    }
  });
}

// ../../initFeatureFlag.module.ts
initFeatureFlag_module_default({});

}();}catch(e){PJS.error(`initFeatureFlag.module`, e);}
optimizely--page-scroll-events.module.js
try{
!function(){
// ../../_includes/oli/optimizely/lifecycle.ts
function onActivated(callback) {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "activated" },
    handler: callback
  });
}

// ../../_includes/oli/optimizely/events.ts
var sendEvent = (eventName, tags2 = {}) => {
  window.optimizely.push({
    type: "event",
    eventName,
    tags: tags2
  });
};

// @oli:logs:@oli:logs:page-scroll-events.module
var log = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[page-scroll-events.module]"))();

// ../../_includes/cromedics/throttle.ts
var throttle = (func, wait, { leading, trailing } = {}) => {
  let context;
  let args;
  let result;
  let timeout = null;
  let previous = 0;
  const later = function() {
    previous = leading === false ? 0 : Date.now();
    timeout = null;
    result = func.apply(context, args);
    if (!timeout)
      context = args = null;
  };
  return function() {
    const now = Date.now();
    if (!previous && leading === false)
      previous = now;
    const remaining = wait - (now - previous);
    context = this;
    args = arguments;
    if (remaining <= 0 || remaining > wait) {
      if (timeout) {
        clearTimeout(timeout);
        timeout = null;
      }
      previous = now;
      result = func.apply(context, args);
      if (!timeout)
        context = args = null;
    } else if (!timeout && trailing !== false) {
      timeout = setTimeout(later, remaining);
    }
    return result;
  };
};
var throttle_default = throttle;

// ../../_includes/cromedics/scroll-values.ts
var getDocumentHeight = () => Math.max(
  document.documentElement.offsetHeight,
  document.documentElement.scrollHeight,
  document.body.offsetHeight,
  document.body.scrollHeight
);
var getMaxScrollPosition = () => getDocumentHeight() - window.innerHeight;
var getScrollPosition = () => Math.max(
  window.pageYOffset,
  document.documentElement.scrollTop,
  document.body.scrollTop
);
var getScrollPercentage = () => getScrollPosition() / getMaxScrollPosition();

// ../../_projectjs/optimizely/page-scroll-events.module.ts
var tags = /* @__PURE__ */ new Map();
var triggerScrollGoals = (tag, percentages) => {
  const scrollPercentage = getScrollPercentage() * 100;
  for (const percent of percentages) {
    const { fired, breakpoint, callback } = percent;
    if (fired)
      continue;
    if (scrollPercentage >= breakpoint) {
      callback(tag, breakpoint);
      percent.fired = true;
    }
  }
};
var sendTagEvent = (tag, percent) => {
  sendEvent(`pjs_${tag}_scroll_${percent}`);
};
function trackPageScroll(tag, breakpoints = [1, 25, 50, 75, 100], callback = sendTagEvent) {
  setTimeout(() => {
    if (tags.has(tag))
      return;
    log(`Tracking "${tag}" scroll metrics.`);
    tags.set(tag, breakpoints.map((breakpoint) => ({
      fired: false,
      breakpoint,
      callback
    })));
  });
}
function page_scroll_events_module_default() {
  window.addEventListener("scroll", throttle_default(() => {
    for (const [tag, percentages] of tags.entries()) {
      triggerScrollGoals(tag, percentages);
    }
  }, 250));
  PJS.utils.trackPageScroll = trackPageScroll;
  onActivated(() => {
    log(`Resetting scroll tracking.`);
    tags.clear();
    trackPageScroll("sitewide");
  });
}

// ../../optimizely__page-scroll-events.module.ts
page_scroll_events_module_default({});

}();}catch(e){PJS.error(`optimizely/page-scroll-events.module`, e);}
optimizely--returning-visitors-segmentation.module.js
try{
!function(){
// ../../_includes/oli/optimizely/lifecycle.ts
function onActivated(callback) {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "activated" },
    handler: callback
  });
}

// ../../_includes/oli/optimizely/attributes.ts
var setAttributes = (attributes, eventName = "push_attribute") => {
  window.optimizely.push({ type: "user", attributes });
  window.optimizely.push({ type: "event", eventName });
};

// ../../_projectjs/optimizely/returning-visitors-segmentation.module.ts
function returning_visitors_segmentation_module_default() {
  onActivated(() => {
    const visitorSession = window.optimizely.get("visitor").first_session ? "New" : "Returning";
    setAttributes({ pjs_returning_visitor: visitorSession });
  });
}

// ../../optimizely__returning-visitors-segmentation.module.ts
returning_visitors_segmentation_module_default({});

}();}catch(e){PJS.error(`optimizely/returning-visitors-segmentation.module`, e);}
pageViewAttributes.module.js
try{
!function(){
// ../../_includes/oli/optimizely/attributes.ts
var setAttributes = (attributes, eventName = "push_attribute") => {
  window.optimizely.push({ type: "user", attributes });
  window.optimizely.push({ type: "event", eventName });
};

// ../../_includes/oli/optimizely/pages.ts
var onPageActivated = (callback, pageId) => {
  window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "pageActivated" },
    handler: (event) => {
      if (!pageId || event.data.page.id === String(pageId))
        callback(event.data);
    }
  });
};

// _projectjs/pageViewAttributes.module.ts
var CHECKOUT_PAGE_AEM = "17287236501";
function pageViewAttributes_module_default() {
  onPageActivated(() => {
    const returnCustomerAttr = window.optimizely.get("visitor").custom["22521880007"];
    if (returnCustomerAttr && returnCustomerAttr.value === "Returning") {
      setAttributes({ pjs_pageview_guest_returning_checkout_page: true });
    } else {
      setAttributes({ pjs_pageview_guest_returning_checkout_page: false });
    }
  }, CHECKOUT_PAGE_AEM);
}

// ../../pageViewAttributes.module.ts
pageViewAttributes_module_default({});

}();}catch(e){PJS.error(`pageViewAttributes.module`, e);}
recommender-utils.js
try{
!function(TAG){
const tag="recommender-utils";

/* eslint-disable prefer-rest-params */

const log = function () {
  return PJS.log(`[${tag}]`, ...arguments);
};

const error = function () {
  return PJS.error(tag, ...arguments);
};

const onInitialized = (callback) => {
  (window.optimizely.initialized && callback()) || window.optimizely.push({
    type: 'addListener',
    filter: { type: 'lifecycle', name: 'initialized' },
    handler: callback,
  });
};

const validDomains = ['www.toryburch.com', 'toryburch.com', 'com.qa2.aem.toryburch.com', 'com.stg.aem.toryburch.com', 'com.uat.aem.toryburch.com', 'na.preview.prod.aem.toryburch.com'];

class FeatureBase {
  constructor(experimentId, tag = `Experiment ID:${experimentId}`, keyPrefix = 'bucketing') {
    if (!validDomains.includes(window.location.hostname)) throw new Error('This can only be run on select domains.');

    this.keyPrefix = keyPrefix;
    this.experimentId = experimentId;
    this.experimentData = window.optimizely.get('data').experiments[this.experimentId];
    this.tag = tag;

    const variationIndex = this.variationIndex = this.getVariationIndex();
    this.setStoredVariationIndex(variationIndex);
  }

  /**
   * The key used for localStorage
   */
  get localStorageKey() {
    return `cro_${this.keyPrefix}_${this.experimentData?.id}`;
  }

  getStoredVariationIndex() {
    return window.localStorage.getItem(this.localStorageKey);
  }

  setStoredVariationIndex(variationIndex) {
    window.localStorage.setItem(this.localStorageKey, variationIndex);
  }

  getOptimizelyForcedIndex() {
    const forceParameter = new URLSearchParams(window.location.search).get('optimizely_x');
    if (forceParameter !== null) {
      const forceVariation = this.experimentData.variations.findIndex((o) => o.id === forceParameter.split(',')[0]);
      return forceVariation > -1 ? forceVariation : 0;
    }
    return null;
  }

  /**
   * Gets the variation index that should be displayed to the visitor (0 = v0, 1 = v1, etc).
   * In cro_mode=qa, if there is a force variation link, we force the variation. If invalid, return control.
   * Leverages both the force query params (used by the client) and our localStorage key used for persistence.
   * If no index is provided by these methods a random variation index will be returned.
   */
  getVariationIndex() {
    const forcedIndex = this.getOptimizelyForcedIndex();
    if (forcedIndex !== null) return forcedIndex;

    const storedIndex = this.getStoredVariationIndex();
    if (storedIndex !== null) return storedIndex;

    const numVariations = this.experimentData.variations ? this.experimentData.variations.length : 2;
    return Math.floor(Math.random() * numVariations);
  }

  /**
   * @param {number?} forceVariation [optional] index of the variation you want to force. 0 = v0, 1 = v1, etc
   */
  bucketVisitor() {
    log(`Bucketing ${this.tag} into v${this.variationIndex}.`);

    // Note: This needs to be run on every applicable page
    window.optimizely.push({
      type: 'bucketVisitor',
      experimentId: this.experimentId,
      variationId: this.experimentData.variations[this.variationIndex]?.id,
    });
  }
}

/**
 *
 * Creates a personalized AB test for Tory Burch.  Essentially just creates a number between 1-2 to determine whether or not to set the personalization test.
 * Force buckets user into v0 or v1 of a campaign for results analysis.
 *
 * @param {string} experimentId Eg. 20335204657
 * @param {number} recommenderType - Recommender to be updated, accepts cartRecommenderName, minicartRecommenderName, or pdpRecommenderName
 * @param {string} recommenderName - Name of recommender (provided by TB)
 */
 
PJS.recommenderObject = {
  recommenders: {},
};
 
class Recommender extends FeatureBase {
  constructor(recommenderType, recommenderName, experimentId, tag) {
    super(experimentId, tag, 'recommender');
    if (!recommenderType) throw new Error(`Invalid recommender type: ${recommenderType}`);
    if (!recommenderName) throw new Error(`Invalid recommender name: ${recommenderName}`);
 
    this.recommenderType = recommenderType;
    this.recommenderName = recommenderName;
  }
 
  activate() {
    super.bucketVisitor();
 
    // If v1 is active, activate the recommender change
    if (this.variationIndex > 0) {
      log(`[${this.tag}] Activating ${this.recommenderName} recommender!`);
      PJS.recommenderObject.recommenders[this.recommenderType] = this.recommenderName;
      window.localStorage.recObject = PJS.recommenderObject;
 
      if (!window.__AB_FEATURES__) window.__AB_FEATURES__ = {};
      window.__AB_FEATURES__.requestAbJson = ({ pathname, aemJson }) => {
        log(`pathname2`, pathname);
        log(`aemJson2`, aemJson);
 
        return PJS.recommenderObject;
      };
    } else {
      log(`[${this.tag}] Deactivating ${this.recommenderName} recommender!`);
    }
  }
}

/**
 * @desc set() sets a cookie with optional days
 *  @param {String} name - the name of the cookie
 *  @param {String} value - the value of the cookie
 *  @param {Number} optDays - days the cookie will exist for
 *    NOTE: Not passing optDays will create a "Session Cookie"
 *  @param {String} domain - the domain value of the cookie
 *    Example: ".domain.com" would span all sub domains of domain.com
 *  @return {Undefined}
 */

/**
 * @desc get() gets value of cookie
 *  @param {String} name - name of cookie to get
 *  @return {String|Null} - string value of cookie NOT A BOOL!
 *
 */
const getCookie = (name) => {
  const nameEQ = `${name}=`;
  const ca = document.cookie.split(';');
  for (let i = 0; i < ca.length; i++) {
    const c = ca[i].trim();
    if (c.indexOf(nameEQ) === 0) return c.substring(nameEQ.length, c.length);
  }
  return null;
};

/**
 * @format 1.0.0
 *
 * @description Utility functions for modifying the product recommender section.
 *
 */

// Note: This can accessed from experiment code using `utils.setRecommender` as well.
PJS.utils.setRecommender = (recommenderType, recommenderName) => {
  PJS.recommenderObject = {
    recommenders: {},
  };

  if (recommenderType === 'custom') {
    PJS.recommenderObject[':items'] = {
      content: {
        ':items': {
          recommendations_caro: {
            recommendationSource: {
              recommendationSourceType: 'recommenderName',
              config: {
                recommenderName,
              },
            },
          },
        },
      },
    };
    log(`Set Custom Recommender to ${recommenderName}`);
  } else {
    if (!PJS.recommenderObject.recommenders) PJS.recommenderObject.recommenders = {};
    PJS.recommenderObject.recommenders[recommenderType] = recommenderName;
    log(`Set "${recommenderType}" Recommender to "${recommenderName}"`);
  }

  window.localStorage.recObject = PJS.recommenderObject;

  if (!window.__AB_FEATURES__) window.__AB_FEATURES__ = {};
  window.__AB_FEATURES__.requestAbJson = ({ pathname, aemJson }) => {
    log(`pathname`, pathname);
    log(`aemJson`, aemJson);

    return PJS.recommenderObject;
  };
};

/* if (!window.__AB_FEATURES__) window.__AB_FEATURES__ = {};
 // eslint-disable-next-line no-unused-vars
 window.__AB_FEATURES__.requestAbJson = ({ pathname, aemJson }) => {
   const recommendations = recommenderObject;
   recommenderObject = {}; // Reset the object after sending the package, preparing for the next SPA transition.
   log(`recommendations`, recommendations);
   return recommendations;
 };  */

const INTEGRATION_ID = '21810380082'; // Tory Burch Integration ID in Opty

/**
   * This will set the recommender values based on values provided in the custom integration
   */
onInitialized(() => {
  const { campaigns } = window.optimizely.get('data');
  for (const campaign of Object.values(campaigns)) {
    try {
      const audience = campaign.experiments?.[0].audienceName;
      if (audience.includes(`QA`) && !(window.location.search.includes(`cro_mode=qa`) || getCookie(`cro_mode`) === 'qa')) {
        PJS.log(`Audience not matched: aborting`);
      } else {
        const experimentID = campaign.experiments?.[0].id;
        const recommenderType = campaign?.integrationSettings?.[INTEGRATION_ID]?.recommenderType;
        const recommenderName = campaign?.integrationSettings?.[INTEGRATION_ID]?.recommenderName;
        const experimentTag = campaign?.integrationSettings?.[INTEGRATION_ID]?.experimentTag;  // Experiment tag is optional

        if (experimentID && recommenderType && recommenderName) {
          const rec = new Recommender(recommenderType, recommenderName, experimentID, experimentTag);
          rec.activate();
        }
      }
    } catch (e) {
      error(e);
    }
  }
});

}(`recommender-utils`);}catch(e){PJS.error(`recommender-utils`, e);}
recommender-utils.module.js
try{
!function(){
// ../../_includes/cromedics/cookies.ts
function getCookie(name) {
  const nameEQ = `${name}=`;
  const cookieArray = document.cookie.split(";");
  for (const cookie of cookieArray) {
    const cookieName = cookie.trim();
    if (cookieName.indexOf(nameEQ) === 0)
      return cookieName.substring(nameEQ.length, cookieName.length);
  }
  return null;
}

// ../../_includes/oli/optimizely/lifecycle.ts
function onInitialized(callback) {
  window.optimizely.initialized && callback() || window.optimizely.push({
    type: "addListener",
    filter: { type: "lifecycle", name: "initialized" },
    handler: callback
  });
}

// @oli:logs:@oli:logs:recommender-utils.module
var log = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[recommender-utils.module]"))();
var error2 = /* @__PURE__ */ (() => window.CRO_PJS.error.bind(window, "recommender-utils.module"))();

// @oli:logs:@oli:logs:Recommender
var log2 = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[Recommender]"))();

// @oli:logs:@oli:logs:FeatureBase
var log3 = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[FeatureBase]"))();

// _includes/FeatureBase.ts
var validDomains = ["www.toryburch.com", "toryburch.com", "com.qa2.aem.toryburch.com", "com.stg.aem.toryburch.com", "com.uat.aem.toryburch.com", "na.preview.prod.aem.toryburch.com"];
var FeatureBase = class {
  constructor(experimentId, tag = `Experiment ID:${experimentId}`, keyPrefix = "bucketing") {
    if (!validDomains.includes(window.location.hostname))
      throw new Error("This can only be run on select domains.");
    this.keyPrefix = keyPrefix;
    this.experimentId = experimentId;
    this.experimentData = window.optimizely.get("data").experiments[this.experimentId];
    this.numVariations = this.experimentData.variations ? this.experimentData.variations.length : 2;
    this.tag = tag;
    const variationIndex = this.variationIndex = this.getVariationIndex();
    this.setStoredVariationIndex(variationIndex);
  }
  /**
   * The key used for localStorage
   */
  get localStorageKey() {
    return `cro_${this.keyPrefix}_${this.experimentData?.id}`;
  }
  getStoredVariationIndex() {
    const index = window.localStorage.getItem(this.localStorageKey);
    return index !== null ? parseInt(index) : null;
  }
  setStoredVariationIndex(variationIndex) {
    window.localStorage.setItem(this.localStorageKey, String(variationIndex));
  }
  getOptimizelyForcedIndex() {
    const forceParameter = new URLSearchParams(window.location.search).get("optimizely_x");
    if (forceParameter !== null) {
      const forceVariation = this.experimentData.variations.findIndex((o) => o.id === forceParameter.split(",")[0]);
      return forceVariation > -1 ? forceVariation : 0;
    }
    return null;
  }
  /**
   * Gets the variation index that should be displayed to the visitor (0 = v0, 1 = v1, etc).
   * In cro_mode=qa, if there is a force variation link, we force the variation. If invalid, return control.
   * Leverages both the force query params (used by the client) and our localStorage key used for persistence.
   * If no index is provided by these methods a random variation index will be returned.
   */
  getVariationIndex() {
    const forcedIndex = this.getOptimizelyForcedIndex();
    if (forcedIndex !== null)
      return forcedIndex;
    const storedIndex = this.getStoredVariationIndex();
    if (storedIndex !== null)
      return storedIndex;
    return Math.floor(Math.random() * this.numVariations);
  }
  /**
   * @param {number?} forceVariation [optional] index of the variation you want to force. 0 = v0, 1 = v1, etc
   */
  bucketVisitor() {
    log3(`Bucketing ${this.tag} into v${this.variationIndex}.`);
    window.optimizely.push({
      type: "bucketVisitor",
      experimentId: `${this.experimentId}`,
      variationId: `${this.experimentData.variations[this.variationIndex]?.id}`
    });
  }
};

// _includes/Recommender.ts
PJS.recommenderObject = {
  recommenders: {}
};
var Recommender = class extends FeatureBase {
  constructor(recommenderType, recommenderName, experimentId, tag) {
    super(experimentId, tag, "recommender");
    if (!recommenderType)
      throw new Error(`Invalid recommender type: ${recommenderType}`);
    if (!recommenderName)
      throw new Error(`Invalid recommender name: ${recommenderName}`);
    this.recommenderType = recommenderType;
    this.recommenderName = recommenderName;
  }
  activate() {
    super.bucketVisitor();
    if (this.variationIndex > 0) {
      log2(`[${this.tag}] Activating ${this.recommenderName} recommender!`);
      PJS.recommenderObject.recommenders[this.recommenderType] = this.recommenderName;
      window.localStorage.recObject = PJS.recommenderObject;
      if (!window.__AB_FEATURES__)
        window.__AB_FEATURES__ = {};
      window.__AB_FEATURES__.requestAbJson = ({ pathname, aemJson }) => {
        log2(`pathname2`, pathname);
        log2(`aemJson2`, aemJson);
        return PJS.recommenderObject;
      };
    } else {
      log2(`[${this.tag}] Deactivating ${this.recommenderName} recommender!`);
    }
  }
};

// _projectjs/recommender-utils.module.ts
function recommender_utils_module_default() {
  PJS.utils.setRecommender = (recommenderType, recommenderName) => {
    PJS.recommenderObject = {
      recommenders: {}
    };
    if (recommenderType === "custom") {
      PJS.recommenderObject[":items"] = {
        content: {
          ":items": {
            recommendations_caro: {
              recommendationSource: {
                recommendationSourceType: "recommenderName",
                config: {
                  recommenderName
                }
              }
            }
          }
        }
      };
      log(`Set Custom Recommender to ${recommenderName}`);
    } else {
      if (!PJS.recommenderObject.recommenders) {
        PJS.recommenderObject.recommenders = {};
      }
      PJS.recommenderObject.recommenders[recommenderType] = recommenderName;
      log(`Set "${recommenderType}" Recommender to "${recommenderName}"`);
    }
    window.localStorage.recObject = PJS.recommenderObject;
    if (!window.__AB_FEATURES__)
      window.__AB_FEATURES__ = {};
    window.__AB_FEATURES__.requestAbJson = ({ pathname, aemJson }) => {
      log(`pathname`, pathname);
      log(`aemJson`, aemJson);
      return PJS.recommenderObject;
    };
  };
  const INTEGRATION_ID = "21810380082";
  onInitialized(() => {
    if (!Array.isArray(window.optimizely)) {
      const { campaigns } = window.optimizely.get("data");
      for (const campaign of Object.values(campaigns)) {
        try {
          const audience = campaign.experiments?.[0].audienceName;
          if (audience.includes(`QA`) && !(window.location.search.includes(`cro_mode=qa`) || getCookie(`cro_mode`) === "qa")) {
            PJS.log(`Audience not matched: aborting`);
          } else {
            const experimentID = parseInt(campaign.experiments?.[0].id);
            const recommenderType = campaign?.integrationSettings?.[INTEGRATION_ID]?.recommenderType;
            const recommenderName = campaign?.integrationSettings?.[INTEGRATION_ID]?.recommenderName;
            const experimentTag = campaign?.integrationSettings?.[INTEGRATION_ID]?.experimentTag;
            if (experimentID && recommenderType && recommenderName) {
              const rec = new Recommender(recommenderType, recommenderName, experimentID, experimentTag);
              rec.activate();
            }
          }
        } catch (e) {
          error2(e);
        }
      }
    }
  });
}

// ../../recommender-utils.module.ts
recommender_utils_module_default({});

}();}catch(e){PJS.error(`recommender-utils.module`, e);}
revenue-tracking-WIP.module.js
try{
!function(){
// ../../_includes/cromedics/monkey-patch.ts
function monkeyPatchBefore(object, key, callback) {
  const original = object[key];
  object[key] = function(...args) {
    try {
      callback(...args);
    } catch (e) {
      console.error(e);
    }
    return typeof original === "function" && original.apply(this, args);
  };
}

// ../../_includes/cromedics/datalayer.ts
var waitForDataLayer = (dataLayerKey = "dataLayer") => new Promise((resolve) => {
  (function poll() {
    if (!window[dataLayerKey] || typeof window[dataLayerKey].push !== "function")
      return setTimeout(poll, 50);
    return resolve(window[dataLayerKey]);
  })();
});
var onDataLayerPush = (callback, {
  dataLayerKey = "dataLayer",
  withPastData = true
} = {}) => waitForDataLayer(dataLayerKey).then((dataLayer) => {
  if (withPastData) {
    dataLayer.forEach((data) => {
      callback(data);
    });
  }
  monkeyPatchBefore(dataLayer, "push", callback);
});

// @oli:logs:@oli:logs:revenue-tracking-WIP.module
var log = /* @__PURE__ */ (() => window.CRO_PJS.log.bind(window, "[revenue-tracking-WIP.module]"))();

// ../../_includes/cromedics/cookies.ts
function delCookie(name, {
  domain = window.location.hostname.split(".").slice(-2).join(".")
} = {}) {
  let cookie = `${name}=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/`;
  if (domain)
    cookie += `; domain=${domain}`;
  document.cookie = cookie;
}
function cookieExists(param, value) {
  return new RegExp(`(^|; )${param}${value !== void 0 ? `=${value}` : "(=.*)?"}(;|$)`).test(document.cookie);
}

// ../../_includes/oli/optimizely/events.ts
var sendEvent = (eventName, tags = {}) => {
  window.optimizely.push({
    type: "event",
    eventName,
    tags
  });
};

// _includes/productAttribution.ts
var families = {
  tMonogram: ["t-monogram", "t monogram", "tmonogram"],
  fleming: ["fleming"],
  ella: ["ella"],
  eleanor: ["eleanor"],
  leeRadziwill: ["lee radziwill", "lee-radziwill"],
  robinson: ["robinson"],
  kira: ["kira"],
  miller: ["miller"],
  mcgraw: ["mcgraw"],
  bonBon: ["bon bon", "bon-bon"],
  claire: ["claire"],
  minnie: ["minnie"],
  goodLuckTrainer: ["good luck trainer", "good-luck-trainer"],
  ladybugSneaker: ["ladybug sneaker", "ladybug-sneaker"],
  court: ["court"]
};
var categories = {
  sale: ["sale"],
  shoes: ["shoes", "shoe"],
  handbags: ["handbags", "handbag", "hbg"]
};
var subcategories = {
  sneakers: ["sneakers", "sneaker"],
  sandals: ["sandals", "sandal"],
  boots: ["boots", "boot"],
  balletFlats: ["ballet flats", "ballet-flats", "ballet", "flat", "flats", "ballet flat", "ballet-flat", "ballets", "ballets flat", "ballets-flat", "ballets flats"],
  loafers: ["loafers", "loafer"],
  totes: ["totes", "tote"],
  shoulderCrossbodyBags: ["shoulder bags", "shoulder bag", "crossbody bags", "crossbody bag"],
  bucketBags: ["bucket bags", "bucket bag"]
};
var getFamilyAttributeName = (product) => {
  const productName = product.productName?.toLowerCase();
  if (productName) {
    for (const [familyName, familyTags] of Object.entries(families)) {
      for (const tag of familyTags) {
        if (productName.includes(tag))
          return familyName;
      }
    }
  }
  return void 0;
};
var getCategoryAttributeName = (product) => {
  const productName = product.productName?.toLowerCase();
  if (productName) {
    for (const [categoryName, categoryTags] of Object.entries(categories)) {
      for (const tag of categoryTags) {
        if (productName.includes(tag))
          return categoryName;
      }
    }
  }
  return void 0;
};
var getSubcategoryAttributeName = (product) => {
  const productName = product.productName?.toLowerCase();
  if (productName) {
    for (const [subcategoryName, subcategoryTags] of Object.entries(subcategories)) {
      for (const tag of subcategoryTags) {
        if (productName.includes(tag))
          return subcategoryName;
      }
    }
  }
  return void 0;
};

// ../../_includes/oli/optimizely/attributes.ts
var setAttributes = (attributes, eventName = "push_attribute") => {
  window.optimizely.push({ type: "user", attributes });
  window.optimizely.push({ type: "event", eventName });
};

// _projectjs/revenue-tracking-WIP.module.ts
var getTagFromSessionStorageKey = (sessionStorageKey) => {
  const match = sessionStorageKey.match(/^cro_(.+)_items$/);
  return match && match[1] || void 0;
};
var dedupedSessionStorageArray = (sessionStorageKey) => {
  const arr = JSON.parse(sessionStorage.getItem(sessionStorageKey)) || [];
  return arr.filter((elem, index, self) => index === self.indexOf(elem));
};
var PAYMENT_METHOD_EVENTS = {
  AFTERPAY: "pjs_afterpay_revenue_aem",
  PayPal: "pjs_paypal_revenue_aem",
  CREDIT_CARD: "pjs_credit_card_revenue_aem",
  APPLE_PAY: "pjs_apple_pay_revenue_aem"
};
function revenue_tracking_WIP_module_default() {
  onDataLayerPush((data) => {
    if (window.location.pathname.indexOf("/order-confirmation") === -1)
      return;
    if (data.event !== "e_orderConfirmationView")
      return;
    const { order, products } = data;
    if (!order || !products || !products.length)
      return;
    const revenueTags = {
      revenue: order.revenue * 100,
      transactionId: order.transactionId,
      value: products.length
    };
    sendEvent("pjs_revenue_aem", revenueTags);
    const [paymentMethod] = order.paymentMethods;
    const paymentMethodEvent = PAYMENT_METHOD_EVENTS[paymentMethod];
    if (paymentMethodEvent) {
      sendEvent(paymentMethodEvent, revenueTags);
    } else {
      log("No payment method found");
    }
    const productTrackingTags = Object.keys(sessionStorage).reduce((out, sessionStorageKey) => {
      const tag = getTagFromSessionStorageKey(sessionStorageKey);
      if (tag)
        out[tag] = dedupedSessionStorageArray(sessionStorageKey);
      return out;
    }, {});
    log(`Product Tracking Tags:`, productTrackingTags);
    products.forEach((product) => {
      log(`Product Info:`, product);
      const revenue = product.price * 100;
      for (const tag in productTrackingTags) {
        productTrackingTags[tag].forEach((searchItemID) => {
          if (product.id === searchItemID || product.sku === searchItemID) {
            sendEvent(`pjs_${tag}_revenue_aem`, { revenue });
          }
        });
      }
      const taxonomyArray = ["taxonomyL1", "taxonomyL2", "taxonomyL3"];
      taxonomyArray.forEach((thisTaxonomy) => {
        if (product[`${thisTaxonomy}`] !== "") {
          sendEvent(`pjs_${product[thisTaxonomy]}_purchased`, { revenue });
        }
      });
      const familyProduct = getFamilyAttributeName(product);
      const category = getCategoryAttributeName(product);
      const subcategory = getSubcategoryAttributeName(product);
      if (familyProduct)
        setAttributes({ ["User Purchased"]: familyProduct });
      if (category)
        setAttributes({ ["User Purchased"]: category });
      if (subcategory)
        setAttributes({ ["User Purchased"]: subcategory });
    });
    if (cookieExists("utm_campaign") && cookieExists("utm_term")) {
      delCookie("utm_campaign");
      delCookie("utm_term");
    }
  }, { dataLayerKey: "gtmDataLayer" });
}

// ../../revenue-tracking-WIP.module.ts
revenue_tracking_WIP_module_default({});

}();}catch(e){PJS.error(`revenue-tracking-WIP.module`, e);}
revenue-tracking.js
try{
!function(TAG){
/**
 * @desc patchDataLayer patches window.dataLayer.push and will call
 * a provided callback with each value that passes through the dataLayer.
 *
 * @param {(data)=>void} callback is sent each data object in the dataLayer array.
 * @param {string} dataLayerKey is the name of the dataLayer variable.
 *   Default is `dataLayer` for window.dataLayer
 */
const patchDataLayer = (callback, dataLayerKey = 'dataLayer')=>{
  (function poll() {
    const original = window[dataLayerKey] && window[dataLayerKey].push;
    if (typeof original !== 'function') return setTimeout(poll, 50);
    //There might already be some data in the dataLayer
    window[dataLayerKey].forEach(data=>{
      callback(data); //Send preexisting data to the callback
    });
    window[dataLayerKey].push = function(...args){
      try {
        callback(...args); //Send any new data to the callback
      } catch(e) {
        console.error(e);
      }
      return original.apply(window[dataLayerKey], args);
    };
  })();
};

const tag="revenue-tracking";

/**
 * Send a custom event to Optimizely
 */
const sendEvent = (eventName, tags = {}) => {
  window.optimizely.push({
    type: "event",
    eventName,
    tags
  });
};

/* eslint-disable prefer-rest-params */

const log = function () {
  return PJS.log(`[${tag}]`, ...arguments);
};

/**
 * @desc set() sets a cookie with optional days
 *  @param {String} name - the name of the cookie
 *  @param {String} value - the value of the cookie
 *  @param {Number} optDays - days the cookie will exist for
 *    NOTE: Not passing optDays will create a "Session Cookie"
 *  @param {String} domain - the domain value of the cookie
 *    Example: ".domain.com" would span all sub domains of domain.com
 *  @return {Undefined}
 */

/**
 * @desc del() removes cookie
 *  @param {String} name - name of cookie to delete
 *  @return {Undefined}
 */
const delCookie = (name, domain = `.${window.location.hostname.split('.').slice(-2).join('.')}`) => {
  let cookie = `${name}=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/`;
  if (domain) {
    cookie += `; domain=${domain}`;
  }
  document.cookie = cookie;
};

const cookieExists = (param, value) => new RegExp(`(^|; )${param}${value !== undefined ? `=${value}` : '(=.*)?'}(;|$)`).test(document.cookie);

/**
 * @format 1.0.0
 *
 * @description
 * This module listens to the GTM dataLayer events on the order confirmation page.
 * It looks for the `e_orderConfirmationView` event which contains the list of products that were purchased.
 *
 * It uses sessionStorage keys to reference products that were added to cart as part of a particular experiment.
 * Session storage keys are of the form: `cro_${tag}_items` where tag is the experiment tag like `tb123` for example.
 * The value is a JSON formatted array of product id or product skus.
 *
 * The module will fire off a custom revenue metric for each matching product that was just purchased.
 * The metric will automatically be of the form `pjs_${tag}_revenue_aem` where the `tag` is taken from the session storage key automatically.
 *   ```
 *
 * @goals
 *  - [PJS] Revenue (AEM) <pjs_revenue_aem>
 *  - [PJS] Afterpay Revenue (AEM) <pjs_afterpay_revenue_aem>
 *  - [PJS] Paypal Revenue (AEM) <pjs_paypal_revenue_aem>
 *  - [PJS] Credit Card Revenue (AEM) <pjs_credit_card_revenue_aem>
 *  - [PJS] Apple Pay Revenue (AEM) <pjs_apple_pay_revenue_aem>
 *  - Dynamic per experiment <pjs_${tag}_revenue_aem>
 *
 * @notes
 * - Create the new metric in Optimizely of the formatL `pjs_${tag}_revenue_aem` where tag is the experiment tag like `tb123`.
 * - Set the sessionStorage value in your experiment code, something like this:
 *   ```
 *   sessionStorage.setItem(`cro_${TAG}_items`, JSON.stringify([12345, 54321]));
 *
 */

/**
  * Parse the experiment tag out of the session storage key.
  * It looks for keys of the format `cro_tb123_items` and returns `tb123` in this case.
  * This returns `undefined` if the string isn't of this format.
  */
const getTagFromSessionStorageKey = (sessionStorageKey) => {
  const match = sessionStorageKey.match(/^cro_(.+)_items$/);
  return (match && match[1]) || undefined;
};

/**
  * Parses the list of items provided in sessionStorage, removes duplicates, then returns the array.
  * The array should be a list of product ids or sku values.
  */
const dedupedSessionStorageArray = (sessionStorageKey) => {
  const arr = JSON.parse(sessionStorage.getItem(sessionStorageKey)) || [];
  return arr.filter((elem, index, self) => index === self.indexOf(elem)); // Dedupe the array
};

const PAYMENT_METHOD_EVENTS = {
  AFTERPAY: 'pjs_afterpay_revenue_aem',
  PayPal: 'pjs_paypal_revenue_aem',
  CREDIT_CARD: 'pjs_credit_card_revenue_aem',
  APPLE_PAY: 'pjs_apple_pay_revenue_aem',
};

patchDataLayer((data) => {
  if (window.location.pathname.indexOf('/order-confirmation') === -1) return;
  if (data.event !== 'e_orderConfirmationView') return;
  const { order, products } = data;
  if (!order || !products || !products.length) return;

  const revenueTags = {
    revenue: order.revenue * 100,
    transactionId: order.transactionId,
    value: products.length,
  };

  sendEvent('pjs_revenue_aem', revenueTags);

  // `paymentMethods` is an array, typically containing only one string value.
  // We assume the first element is the only payment method being used.
  const [paymentMethod] = order.paymentMethods;
  const paymentMethodEvent = PAYMENT_METHOD_EVENTS[paymentMethod];
  if (paymentMethodEvent) {
    sendEvent(paymentMethodEvent, revenueTags);
  } else {
    log('No payment method found');
  }

  const productTrackingTags = Object.keys(sessionStorage).reduce((out, sessionStorageKey) => {
    const tag = getTagFromSessionStorageKey(sessionStorageKey); // An experiment tag like `tb123`
    if (tag) out[tag] = dedupedSessionStorageArray(sessionStorageKey);
    return out;
  }, {});
  log(`Product Tracking Tags:`, productTrackingTags);

  products.forEach((product) => {
    log(`Product Info:`, product);
    const revenue = product.price * 100;

    for (const tag in productTrackingTags) {
      productTrackingTags[tag].forEach((searchItemID) => {
        if (product.id === searchItemID || product.sku === searchItemID) {
          sendEvent(`pjs_${tag}_revenue_aem`, { revenue });
        }
      });
    }
  });

  if (cookieExists('utm_campaign') && cookieExists('utm_term')) {
    delCookie('utm_campaign');
    delCookie('utm_term');
  }
}, 'gtmDataLayer');

}(`revenue-tracking`);}catch(e){PJS.error(`revenue-tracking`, e);}
userAgent_attribute.module.js
!function(){
// ../../_includes/cromedics/user-agents.ts
var knownBots = ["PhantomJS", "Prerender", "HeadlessChrome", "PetalBot", "Checkly"];
var isKnownBot = (userAgent = window.navigator.userAgent) => knownBots.some((botName) => userAgent.includes(botName));

// ../../_includes/oli/optimizely/attributes.ts
var setAttributes = (attributes, eventName = "push_attribute") => {
  window.optimizely.push({ type: "user", attributes });
  window.optimizely.push({ type: "event", eventName });
};

// _projectjs/userAgent_attribute.module.ts
function userAgent_attribute_module_default() {
  const { userAgent } = window.navigator;
  setAttributes({ pjs_useragent: userAgent });
  if (isKnownBot(userAgent) || userAgent.includes("ToryBurchAutotest")) {
    setAttributes({ pjs_disabled_bot: "true" });
    throw "bot traffic detected.";
  }
}

// ../../userAgent_attribute.module.ts
userAgent_attribute_module_default({});

}();
@watangwa
Comment
 
Leave a comment
 
Footer
© 2024 GitHub, Inc.
Footer navigation
Terms
