# Inserted-JS-on-Treehouse-Lesson
(function() {
    'use strict';

    var e = /\s/g
      , t = /\W/g
      , n = {
        plain: function(e) {
            return e
        },
        hyphens: function(n) {
            return n.trim().replace(e, "-").replace(t, "-")
        }
    };
    var r = function(e, t) {
        var n = t.zero
          , r = t.one;
        return "{" + e + ", plural,\n" + (n ? "\t=0{" + n + "}\n" : "") + (r ? "\tone{" + r + "}\n" : "") + "\tother{" + t.other + "}}"
    }
      , i = {
        "&": "&amp;",
        "<": "&lt;",
        ">": "&gt;",
        '"': "&quot;",
        "'": "&#39;"
    }
      , o = Object.defineProperty({
        generator: n,
        Plural: r,
        splitAndEscapeReplacements: function(e) {
            var t = {}
              , n = {};
            for (var r in e)
                if (e.hasOwnProperty(r)) {
                    var o = e[r];
                    "function" == typeof o ? n[r] = o : t[r] = "string" == typeof o ? o.replace(/[&<>"']/g, (function(e) {
                        return i[e]
                    }
                    )) : o;
                }
            return [t, n]
        },
        assign: function(e, t) {
            var n = Object(e);
            for (var r in t)
                Object.prototype.hasOwnProperty.call(t, r) && (n[r] = t[r]);
            return n
        }
    }, "__esModule", {
        value: !0
    })
      , a = "undefined" != typeof globalThis ? globalThis : "undefined" != typeof window ? window : "undefined" != typeof global ? global : "undefined" != typeof self ? self : {};
    var u = function(e) {
        var t = {};
        return function(n, r) {
            var i = Array.prototype.slice.call(arguments)
              , o = n + "-" + JSON.stringify(r);
            if (o in t)
                return t[o];
            var a, u = new ((a = Function.prototype.bind).call.apply(a, [e, null].concat(i)));
            return t[o] = u,
            u
        }
    }
      , c = Object.defineProperty({
        dateTimeFormats: {
            short: {
                month: "short",
                day: "numeric",
                year: "numeric"
            },
            long: {
                month: "long",
                day: "numeric",
                year: "numeric"
            },
            dateTime: {
                month: "short",
                day: "numeric",
                year: "numeric",
                hour: "numeric",
                minute: "numeric"
            }
        },
        numberFormats: {
            currency: {
                style: "currency",
                currency: "USD"
            },
            decimal: {
                style: "decimal"
            },
            percent: {
                style: "percent"
            }
        },
        default: u
    }, "__esModule", {
        value: !0
    })
      , s = "{"
      , l = "}";
    var d, f = function(e, t) {
        if (!t)
            return e;
        for (var n = "", r = [], i = 0; i < e.length; i++)
            switch (e.charAt(i)) {
            case s:
                r.push(n),
                n = "";
                break;
            case l:
                r.push(t[n]),
                n = "";
                break;
            default:
                n += e.charAt(i);
            }
        return r.push(n),
        r.join("")
    }, p = Object.defineProperty({
        default: f
    }, "__esModule", {
        value: !0
    });
    var v = function(e, t) {
        void 0 === d && (d = new DOMParser);
        var n = d.parseFromString("<wrap>" + e + "</wrap>", "text/xml");
        if (!n.firstChild || "wrap" !== n.firstChild.nodeName)
            throw new Error("Could not parse XML string");
        return h(n.firstChild, t, !0)
    };
    function h(e, t, n) {
        if (void 0 === n && (n = !1),
        3 === e.nodeType)
            return e.nodeValue ? [e.nodeValue] : [];
        if (1 === e.nodeType) {
            var r = Array.prototype.slice.call(e.childNodes).reduce((function(e, n) {
                return e.concat(h(n, t))
            }
            ), []);
            return n || !t[e.nodeName] ? r : [t[e.nodeName].apply(t, r)]
        }
        return []
    }
    var m = Object.defineProperty({
        default: v
    }, "__esModule", {
        value: !0
    })
      , y = function(e) {
        var t = {
            exports: {}
        };
        return e(t, t.exports),
        t.exports
    }((function(e, t) {
        Object.defineProperty(t, "__esModule", {
            value: !0
        });
        t.makeBasicT = function() {
            var e = {}
              , t = "en"
              , n = o.generator.hyphens
              , r = function(e) {
                return n(e)
            }
              , i = function(n, r, i) {
                void 0 === r && (r = {}),
                void 0 === i && (i = "");
                var o = function(e, t, n) {
                    var r = e[t]
                      , i = e.en;
                    return r && r[n] ? r[n] : i && i[n] ? i[n] : ""
                }(e, t, n) || i || n;
                return "string" == typeof o ? p.default(o, r) : o(r)
            }
              , a = function(e, t, n) {
                return void 0 === t && (t = {}),
                void 0 === n && (n = ""),
                i(n || r(e), t, e)
            }
              , u = {
                $: function(e, t, n) {
                    void 0 === t && (t = {}),
                    void 0 === n && (n = "");
                    var r = o.splitAndEscapeReplacements(t)
                      , i = r[0]
                      , u = r[1]
                      , c = a(e, i, n);
                    return m.default(c, u)
                },
                generateId: r,
                locale: function() {
                    return t
                },
                lookup: i,
                set: function(r) {
                    return void 0 === r && (r = {}),
                    e = r.messages || e,
                    t = r.locale || t,
                    n = r.idGenerator || n,
                    {
                        messages: e,
                        locale: t,
                        idGenerator: n
                    }
                }
            };
            return o.assign(a, u)
        }
        ,
        t.makeT = function() {
            var e = t.makeBasicT()
              , n = function(e) {
                if ("undefined" == typeof Intl) {
                    var t = function() {
                        throw new Error("Missing Intl")
                    };
                    return {
                        date: t,
                        number: t
                    }
                }
                try {
                    Intl.DateTimeFormat(),
                    (new Date).toLocaleString(),
                    (new Date).toLocaleDateString(),
                    (new Date).toLocaleTimeString();
                } catch (e) {
                    Date.prototype.toLocaleString = Date.prototype.toString,
                    Date.prototype.toLocaleDateString = Date.prototype.toDateString,
                    Date.prototype.toLocaleTimeString = Date.prototype.toTimeString,
                    Intl.DateTimeFormat = function() {
                        var e = Intl.DateTimeFormat;
                        function t() {
                            var t = Array.prototype.slice.apply(arguments);
                            return t[0] = t[0] || "en-US",
                            t[1] = t[1] || {},
                            t[1].timeZone = t[1].timeZone || "America/Toronto",
                            e.apply(this, t)
                        }
                        return t.prototype = e.prototype,
                        t
                    }();
                }
                var n = c.default(Intl.DateTimeFormat)
                  , r = c.default(Intl.NumberFormat);
                return {
                    date: function(t, r, i) {
                        void 0 === r && (r = "long"),
                        void 0 === i && (i = e());
                        var o = c.dateTimeFormats[r] || c.dateTimeFormats.long;
                        return n(i, o).format(t)
                    },
                    number: function(t, n, i) {
                        void 0 === n && (n = "decimal"),
                        void 0 === i && (i = e());
                        var o = c.numberFormats[n] || c.numberFormats.decimal;
                        return r(i, o).format(t)
                    }
                }
            }(e.locale);
            return o.assign(e, n)
        }
        ,
        t.default = t.makeT();
    }
    ));
    o.Plural,
    o.generator,
    y.default,
    y.makeBasicT;
    var g = y.makeT
      , b = {
        de: {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "Unbekannte Website"
                },
                "auth-type-code-to-fill": function(e) {
                    return e.code + " tippen, um " + e.type + e.website + " auszuf??llen."
                },
                "auth-filling-on-website": function(e) {
                    return " auf " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "Falscher Code eingegeben"
                },
                "auth-why-is-this-needed": function(e) {
                    return "Wieso ist das n??tig?"
                },
                "list-no-items": function(e) {
                    return "Keine anzuzeigenden Objekte vorhanden."
                },
                "item-save-in-1password": function(e) {
                    return "In 1Password speichern"
                },
                "item-use-suggested-password": function(e) {
                    return "Empfohlenes Passwort nutzen"
                },
                "item-create-a-privacy-card": function(e) {
                    return "Privacy Card erstellen"
                },
                "item-type-credit-card": function(e) {
                    return "Kreditkarte"
                },
                "item-type-identity": function(e) {
                    return "Identit??t"
                },
                "item-type-unspecified": function(e) {
                    return "Objekt"
                },
                categories: function(e) {
                    return "Kategorien"
                },
                "category-suggestions": function(e) {
                    return "Vorschl??ge"
                },
                "category-logins": function(e) {
                    return "Logins"
                },
                "category-identities": function(e) {
                    return "Identit??ten"
                },
                "category-credit-cards": function(e) {
                    return "Kreditkarten"
                },
                "category-generated-password": function(e) {
                    return "Generiertes Passwort"
                },
                "category-hide-on-this-page": function(e) {
                    return "Auf dieser Seite ausblenden"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "Bitte 1Password per Symbolleistenicon entsperren."
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return e.shortcut + "-K??rzel tippen, um 1Password zu entsperren"
                },
                "locked-request-unlock": function(e) {
                    return "1Password ??ffnen"
                },
                "notification-add-account": function(e) {
                    return "Konto hinzuf??gen zu"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "Sie k??nnen Konten sp??ter in 1Password hinzuf??gen und entfernen"
                },
                "notification-settings": function(e) {
                    return "Einstellungen"
                },
                "notification-add-account-never": function(e) {
                    return "Niemals"
                },
                "notification-add-account-confirm": function(e) {
                    return "Hinzuf??gen"
                },
                "authorize-fill": function(e) {
                    return "Klicken Sie OK, um Ihr 1Password-Objekt auf " + e.host + " auszuf??llen"
                },
                "authorize-fill-privacy": function(e) {
                    return "Klicken Sie auf OK, um mit 1Password eine Privacy Card auf " + e.host + " zu erstellen und auszuf??llen"
                },
                Title: function(e) {
                    return "Titel"
                },
                "Save-new-Login": function(e) {
                    return "Neuen Login speichern"
                },
                cancel: function(e) {
                    return "Abbrechen"
                },
                close: function(e) {
                    return "Schlie??en"
                },
                confirm: function(e) {
                    return "Best??tigen"
                },
                Save: function(e) {
                    return "Speichern"
                },
                Update: function(e) {
                    return "Update"
                },
                "unknown-item": function(e) {
                    return "Unbekanntes Objekt"
                },
                "save-in": function(e) {
                    return "Speichern in"
                },
                "select-a-vault": function(e) {
                    return "Tresor w??hlen"
                },
                "locked-title": function(e) {
                    return "1Password ist gesperrt"
                },
                "locked-message": function(e) {
                    return "Um weiter mit 1Password zu speichern, entsperren Sie ein Konto."
                },
                "current-item": function(e) {
                    return "Aktuelles Objekt"
                },
                "updated-item": function(e) {
                    return "Aktualisiertes Objekt"
                },
                "add-tag": function(e) {
                    return "Tag hinzuf??gen"
                },
                "remove-tag": function(e) {
                    return "Tag entfernen"
                },
                "enable-privacy-header": function(e) {
                    return "Zu 1Password hinzuf??gen"
                },
                "enable-privacy-description": function(e) {
                    return "Verwenden Sie 1Password, um ??berall dort, wo Sie online bezahlen, Privacy Cards zu erstellen und auszuf??llen und H??ndlerkarten f??r die zuk??nftige Verwendung zu speichern."
                },
                "enable-privacy-choose-account": function(e) {
                    return "W??hlen Sie ein Konto"
                },
                "enable-privacy-add-button": function(e) {
                    return "Zu 1Password hinzuf??gen"
                },
                "enable-privacy-error": function(e) {
                    return "Die Integration von Privacy konnte nicht aktiviert werden. Bitte sp??ter erneut versuchen."
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "Bitte entsperren Sie das ausgew??hlte Konto, um die Integration zu aktivieren."
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "Bitte entsperren Sie 1Password, um die Integration zu aktivieren."
                },
                "privacy-error-enable-header": function(e) {
                    return "Integration kann nicht aktiviert werden"
                },
                "privacy-error-header": function(e) {
                    return "Es ist ein Fehler aufgetreten"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "F??r die Integration mit Privacy ist eine 1Password-Mitgliedschaft erforderlich."
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "Bitte vergewissern Sie sich, dass mindestens eine Finanzierungsquelle mit Ihrem Privacy.com-Konto verkn??pft ist und versuchen Sie es dann erneut."
                },
                "privacy-error-unexpected-error": function(e) {
                    return "Es ist ein unerwarteter Fehler aufgetreten. Bitte kontaktieren Sie support@1password.com"
                },
                "privacy-error-okay-button": function(e) {
                    return "OK"
                },
                "privacy-once": function(e) {
                    return "Einmalig"
                },
                "privacy-monthly": function(e) {
                    return "Monatlich"
                },
                "privacy-annually": function(e) {
                    return "J??hrlich"
                },
                "privacy-transaction": function(e) {
                    return "Transaktion"
                },
                "privacy-forever": function(e) {
                    return "F??r immer"
                },
                "privacy-header": function(e) {
                    return "Privacy Card erstellen"
                },
                "privacy-card-name-label": function(e) {
                    return "Kartenname"
                },
                "privacy-spend-limit-label": function(e) {
                    return "Ausgabenlimit festlegen"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "Frequenz"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "Betrag"
                },
                "privacy-funding-account-label": function(e) {
                    return "Finanzierungsquelle"
                },
                "privacy-save-label": function(e) {
                    return "In 1Password speichern"
                },
                "privacy-create-button": function(e) {
                    return "Erstellen und ausf??llen"
                },
                "currency-input-aria-label": function(e) {
                    return "Betrag in Dollar eingeben"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "Bitte aktivieren Sie die Privacy-Integration ??ber das Kontextmen?? der Developer Tools."
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password ist gesperrt. Bitte entsperren und erneut versuchen."
                },
                "privacy-error-vault-locked": function(e) {
                    return "Dieser Tresor ist gesperrt. Bitte entsperren und erneut versuchen."
                },
                "privacy-error-save-failed": function(e) {
                    return "Objekt konnte nicht gespeichert werden. Pr??fen Sie, ob der Tresor entsperrt ist, und versuchen Sie es erneut."
                },
                "privacy-error-empty-card-name": function(e) {
                    return "Bitte geben Sie einen Kartenname ein."
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "Bitte geben Sie einen k??rzeren Kartenname ein."
                },
                "privacy-error-empty-limit": function(e) {
                    return "Sie m??ssen ein Ausgabenlimit f??r die Karte angeben."
                },
                "privacy-error-limit-too-large": function(e) {
                    return "Bitte geben Sie ein kleineres Ausgabenlimit f??r die Karte ein."
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "URL kann nicht geparst werden"
                },
                "privacy-error-http-error": function(e) {
                    return "Privacy nicht erreichbar. Bitte ??berpr??fen Sie Ihre Internetverbindung und versuchen Sie es erneut."
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "Authentifizierung mit Privacy nicht m??glich. Bitte ??berpr??fen Sie Ihren API-Schl??ssel und versuchen Sie es erneut."
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "Fehler beim Erstellen der Karte: " + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "Fehler beim Erstellen der Karte."
                },
                "privacy-error-brain-error": function(e) {
                    return "Es ist nicht m??glich, ein Kreditkarten-Objekt von der Privacy Card aus zu erstellen."
                },
                "privacy-error-default": function(e) {
                    return "Ein unerwarteter Fehler ist aufgetreten."
                },
                "notification-save-in": function(e) {
                    return "Speichern in"
                },
                "notification-save-secondary": function(e) {
                    return "Soll Ihr " + e.host + "-Login in 1Password gespeichert werden?"
                },
                "notification-save-save": function(e) {
                    return "Speichern"
                }
            }
        },
        en: {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "Unfamiliar Website"
                },
                "auth-type-code-to-fill": function(e) {
                    return "Type " + e.code + " to authorize " + e.type + " filling" + e.website + "."
                },
                "auth-filling-on-website": function(e) {
                    return " on " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "Incorrect code entered"
                },
                "auth-why-is-this-needed": function(e) {
                    return "Why is this needed?"
                },
                "list-no-items": function(e) {
                    return "No items to show."
                },
                "item-save-in-1password": function(e) {
                    return "Save in 1Password"
                },
                "item-use-suggested-password": function(e) {
                    return "Use Suggested Password"
                },
                "item-create-a-privacy-card": function(e) {
                    return "Create a Privacy Card"
                },
                "item-type-credit-card": function(e) {
                    return "credit card"
                },
                "item-type-identity": function(e) {
                    return "identity"
                },
                "item-type-unspecified": function(e) {
                    return "item"
                },
                categories: function(e) {
                    return "Categories"
                },
                "category-suggestions": function(e) {
                    return "Suggestions"
                },
                "category-logins": function(e) {
                    return "Logins"
                },
                "category-identities": function(e) {
                    return "Identities"
                },
                "category-credit-cards": function(e) {
                    return "Credit Cards"
                },
                "category-generated-password": function(e) {
                    return "Generated Password"
                },
                "category-hide-on-this-page": function(e) {
                    return "Hide on this page"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "Please unlock 1Password from the toolbar icon."
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return "Press " + e.shortcut + " to unlock 1Password"
                },
                "locked-request-unlock": function(e) {
                    return "Open 1Password"
                },
                "notification-add-account": function(e) {
                    return "Add account to"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "You can add and remove accounts later from 1Password"
                },
                "notification-settings": function(e) {
                    return "Settings"
                },
                "notification-add-account-never": function(e) {
                    return "Never"
                },
                "notification-add-account-confirm": function(e) {
                    return "Add"
                },
                "authorize-fill": function(e) {
                    return "Click OK to fill your 1Password item on " + e.host
                },
                "authorize-fill-privacy": function(e) {
                    return "Click OK to create and fill a Privacy Card using 1Password on " + e.host
                },
                Title: function(e) {
                    return "Title"
                },
                "Save-new-Login": function(e) {
                    return "Save new Login"
                },
                cancel: function(e) {
                    return "Cancel"
                },
                close: function(e) {
                    return "Close"
                },
                confirm: function(e) {
                    return "Confirm"
                },
                Save: function(e) {
                    return "Save"
                },
                Update: function(e) {
                    return "Update"
                },
                "unknown-item": function(e) {
                    return "unknown item"
                },
                "save-in": function(e) {
                    return "save in"
                },
                "select-a-vault": function(e) {
                    return "Select a vault"
                },
                "locked-title": function(e) {
                    return "1Password is locked"
                },
                "locked-message": function(e) {
                    return "To continue saving with 1Password, unlock an account."
                },
                "current-item": function(e) {
                    return "Current Item"
                },
                "updated-item": function(e) {
                    return "Updated Item"
                },
                "add-tag": function(e) {
                    return "Add tag"
                },
                "remove-tag": function(e) {
                    return "Remove tag"
                },
                "enable-privacy-header": function(e) {
                    return "Add to 1Password"
                },
                "enable-privacy-description": function(e) {
                    return "Use 1Password to create and fill Privacy Cards everywhere you pay online, and save merchant cards for future use."
                },
                "enable-privacy-choose-account": function(e) {
                    return "Choose an account"
                },
                "enable-privacy-add-button": function(e) {
                    return "Add to 1Password"
                },
                "enable-privacy-error": function(e) {
                    return "Unable to enable the Privacy integration. Please try again later."
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "Please unlock the selected account to enable the integration."
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "Please unlock 1Password to enable the integration."
                },
                "privacy-error-enable-header": function(e) {
                    return "Unable to enable integration"
                },
                "privacy-error-header": function(e) {
                    return "An error has occurred"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "A 1Password membership is required to integrate with Privacy."
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "Please ensure there is at least one Funding Source associated with your Privacy.com account, then try again."
                },
                "privacy-error-unexpected-error": function(e) {
                    return "An unexpected error occurred. Please contact support@1password.com"
                },
                "privacy-error-okay-button": function(e) {
                    return "OK"
                },
                "privacy-once": function(e) {
                    return "Once"
                },
                "privacy-monthly": function(e) {
                    return "Monthly"
                },
                "privacy-annually": function(e) {
                    return "Annually"
                },
                "privacy-transaction": function(e) {
                    return "Transaction"
                },
                "privacy-forever": function(e) {
                    return "Forever"
                },
                "privacy-header": function(e) {
                    return "Create a Privacy Card"
                },
                "privacy-card-name-label": function(e) {
                    return "Card name"
                },
                "privacy-spend-limit-label": function(e) {
                    return "Set spending limit"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "Frequency"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "Amount"
                },
                "privacy-funding-account-label": function(e) {
                    return "Funding source"
                },
                "privacy-save-label": function(e) {
                    return "Save in 1Password"
                },
                "privacy-create-button": function(e) {
                    return "Create and Fill"
                },
                "currency-input-aria-label": function(e) {
                    return "Enter the amount in dollars"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "Please enable the Privacy integration from the Developer Tools context menu."
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password is locked. Please try again after unlocking."
                },
                "privacy-error-vault-locked": function(e) {
                    return "This vault is locked. Please try again after unlocking."
                },
                "privacy-error-save-failed": function(e) {
                    return "Unable to save item. Check that the vault is unlocked and try again."
                },
                "privacy-error-empty-card-name": function(e) {
                    return "Please enter a name for the card."
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "Please enter a smaller name for the card."
                },
                "privacy-error-empty-limit": function(e) {
                    return "You must provide a spending limit for the card."
                },
                "privacy-error-limit-too-large": function(e) {
                    return "Please enter a smaller spending limit for the card."
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "Unable to parse URL"
                },
                "privacy-error-http-error": function(e) {
                    return "We were unable to reach Privacy. Please check your internet connection and try again."
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "Unable to authenticate with Privacy. Please check your API key and try again."
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "Error creating card: " + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "Error creating card."
                },
                "privacy-error-brain-error": function(e) {
                    return "Unable to create credit card item from Privacy card."
                },
                "privacy-error-default": function(e) {
                    return "An unexpected error occurred."
                },
                "notification-save-in": function(e) {
                    return "Save in"
                },
                "notification-save-secondary": function(e) {
                    return "Save your " + e.host + " login in 1Password?"
                },
                "notification-save-save": function(e) {
                    return "Save"
                }
            }
        },
        es: {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "P??gina web no familiar"
                },
                "auth-type-code-to-fill": function(e) {
                    return "Escribe " + e.code + " para autorizar que " + e.type + " complete" + e.website + "."
                },
                "auth-filling-on-website": function(e) {
                    return " en " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "C??digo incorrecto introducido"
                },
                "auth-why-is-this-needed": function(e) {
                    return "??Por qu?? es esto necesario?"
                },
                "list-no-items": function(e) {
                    return "No hay elementos que mostrar."
                },
                "item-save-in-1password": function(e) {
                    return "Guardar en 1Password"
                },
                "item-use-suggested-password": function(e) {
                    return "Usar contrase??a sugerida"
                },
                "item-create-a-privacy-card": function(e) {
                    return "Crear una tarjeta de Privacy"
                },
                "item-type-credit-card": function(e) {
                    return "tarjeta de cr??dito"
                },
                "item-type-identity": function(e) {
                    return "identidad"
                },
                "item-type-unspecified": function(e) {
                    return "elemento"
                },
                categories: function(e) {
                    return "Categor??as"
                },
                "category-suggestions": function(e) {
                    return "Sugerencias"
                },
                "category-logins": function(e) {
                    return "Inicios de sesi??n"
                },
                "category-identities": function(e) {
                    return "Identidades"
                },
                "category-credit-cards": function(e) {
                    return "Tarjetas de cr??dito"
                },
                "category-generated-password": function(e) {
                    return "Contrase??a generada"
                },
                "category-hide-on-this-page": function(e) {
                    return "Ocultar en esta p??gina"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "Por favor, desbloquea 1Password desde el icono en la barra de herramientas."
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return "Pulsa " + e.shortcut + " para desbloquear 1Password"
                },
                "locked-request-unlock": function(e) {
                    return "Abrir 1Password"
                },
                "notification-add-account": function(e) {
                    return "A??adir cuenta a"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "Puedes a??adir y quitar cuentas m??s tarde desde 1Password"
                },
                "notification-settings": function(e) {
                    return "Ajustes"
                },
                "notification-add-account-never": function(e) {
                    return "Nunca"
                },
                "notification-add-account-confirm": function(e) {
                    return "A??adir"
                },
                "authorize-fill": function(e) {
                    return "Haz clic en OK para completar tu elemento de 1Password en " + e.host
                },
                "authorize-fill-privacy": function(e) {
                    return "Haz clic en OK para crear y cumplimentar una tarjeta de Privacy usando 1Password en " + e.host
                },
                Title: function(e) {
                    return "T??tulo"
                },
                "Save-new-Login": function(e) {
                    return "Guardar nuevo inicio de sesi??n"
                },
                cancel: function(e) {
                    return "Cancelar"
                },
                close: function(e) {
                    return "Cerrar"
                },
                confirm: function(e) {
                    return "Confirmar"
                },
                Save: function(e) {
                    return "Guardar"
                },
                Update: function(e) {
                    return "Actualizar"
                },
                "unknown-item": function(e) {
                    return "elemento desconocido"
                },
                "save-in": function(e) {
                    return "guardar en"
                },
                "select-a-vault": function(e) {
                    return "Selecciona una b??veda"
                },
                "locked-title": function(e) {
                    return "1Password est?? bloqueado"
                },
                "locked-message": function(e) {
                    return "Para continuar guardando con 1Password, desbloquea una cuenta."
                },
                "current-item": function(e) {
                    return "Elemento actual"
                },
                "updated-item": function(e) {
                    return "Elemento actualizado"
                },
                "add-tag": function(e) {
                    return "A??adir etiqueta"
                },
                "remove-tag": function(e) {
                    return "Eliminar etiqueta"
                },
                "enable-privacy-header": function(e) {
                    return "A??adir a 1Password"
                },
                "enable-privacy-description": function(e) {
                    return "Usa 1Password para crear y cumplimentar tarjetas de Privacy siempre que pagues en l??nea, y guarda tarjetas comerciales para usarlas en el futuro."
                },
                "enable-privacy-choose-account": function(e) {
                    return "Elegir una cuenta"
                },
                "enable-privacy-add-button": function(e) {
                    return "A??adir a 1Password"
                },
                "enable-privacy-error": function(e) {
                    return "No se ha podido habilitar la integraci??n de Privacy. Int??ntalo de nuevo m??s tarde."
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "Desbloquea la cuenta seleccionada para habilitar la integraci??n."
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "Desbloquea 1Password para habilitar la integraci??n."
                },
                "privacy-error-enable-header": function(e) {
                    return "No se puede habilitar la integraci??n"
                },
                "privacy-error-header": function(e) {
                    return "Ha ocurrido un error"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "Se requiere una suscripci??n de 1Password para integrarlo con Privacy."
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "Aseg??rate de que hay al menos una fuente de financiaci??n vinculada a tu cuenta de Privacy.com e int??ntalo de nuevo."
                },
                "privacy-error-unexpected-error": function(e) {
                    return "Se ha producido un error inesperado. Ponte en contacto con support@1password.com"
                },
                "privacy-error-okay-button": function(e) {
                    return "OK"
                },
                "privacy-once": function(e) {
                    return "Una vez"
                },
                "privacy-monthly": function(e) {
                    return "Mensual"
                },
                "privacy-annually": function(e) {
                    return "Anual"
                },
                "privacy-transaction": function(e) {
                    return "Transacci??n"
                },
                "privacy-forever": function(e) {
                    return "Para siempre"
                },
                "privacy-header": function(e) {
                    return "Crear una tarjeta de Privacy"
                },
                "privacy-card-name-label": function(e) {
                    return "Nombre de la tarjeta"
                },
                "privacy-spend-limit-label": function(e) {
                    return "Configurar l??mite de gastos"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "Frecuencia"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "Cantidad"
                },
                "privacy-funding-account-label": function(e) {
                    return "Fuente de financiaci??n"
                },
                "privacy-save-label": function(e) {
                    return "Guardar en 1Password"
                },
                "privacy-create-button": function(e) {
                    return "Crear y completar"
                },
                "currency-input-aria-label": function(e) {
                    return "Introduce la cantidad en d??lares"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "Habilita la integraci??n de Privacy desde el men?? contextual de las herramientas del desarrollador."
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password est?? bloqueado. Por favor int??ntalo de nuevo despu??s de desbloquearlo."
                },
                "privacy-error-vault-locked": function(e) {
                    return "Esta b??veda est?? bloqueada. Int??ntalo de nuevo despu??s de desbloquearla."
                },
                "privacy-error-save-failed": function(e) {
                    return "No se ha podido guardar el elemento. Comprueba que la b??veda est?? desbloqueada y vuelve a intentarlo."
                },
                "privacy-error-empty-card-name": function(e) {
                    return "Introduce un nombre para la tarjeta."
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "Introduce un nombre m??s corto para la tarjeta."
                },
                "privacy-error-empty-limit": function(e) {
                    return "Debes facilitar un l??mite de gastos para la tarjeta."
                },
                "privacy-error-limit-too-large": function(e) {
                    return "Introduce un l??mite de gastos menor para la tarjeta."
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "No se ha podido analizar la URL"
                },
                "privacy-error-http-error": function(e) {
                    return "No nos hemos podido conectar con Privacy. Revisa tu conexi??n a internet e int??ntalo de nuevo."
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "No se ha podido autenticar con Privacy. Comprueba tu clave API y vuelve a intentarlo."
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "Error al crear la tarjeta: " + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "Error al crear la tarjeta."
                },
                "privacy-error-brain-error": function(e) {
                    return "No se ha podido crear el elemento de la tarjeta de cr??dito desde la tarjeta de Privacy."
                },
                "privacy-error-default": function(e) {
                    return "Se ha producido un error inesperado."
                },
                "notification-save-in": function(e) {
                    return "Guardar en"
                },
                "notification-save-secondary": function(e) {
                    return "??Guardar el inicio de sesi??n de " + e.host + " en 1Password?"
                },
                "notification-save-save": function(e) {
                    return "Guardar"
                }
            }
        },
        fr: {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "Site web non familier"
                },
                "auth-type-code-to-fill": function(e) {
                    return "Tapez " + e.code + " pour autoriser " + e.type + " ?? renseigner " + e.website + "."
                },
                "auth-filling-on-website": function(e) {
                    return " sur " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "Code entr?? incorrect"
                },
                "auth-why-is-this-needed": function(e) {
                    return "Pourquoi est-ce n??cessaire ?"
                },
                "list-no-items": function(e) {
                    return "Aucun ??l??ment ?? afficher."
                },
                "item-save-in-1password": function(e) {
                    return "Enregistrer dans 1Password"
                },
                "item-use-suggested-password": function(e) {
                    return "Utiliser le mot de passe sugg??r??"
                },
                "item-create-a-privacy-card": function(e) {
                    return "Cr??er une carte Privacy"
                },
                "item-type-credit-card": function(e) {
                    return "carte de cr??dit"
                },
                "item-type-identity": function(e) {
                    return "identit??"
                },
                "item-type-unspecified": function(e) {
                    return "??l??ment"
                },
                categories: function(e) {
                    return "Cat??gories"
                },
                "category-suggestions": function(e) {
                    return "Suggestions"
                },
                "category-logins": function(e) {
                    return "Identifiants"
                },
                "category-identities": function(e) {
                    return "Identit??s"
                },
                "category-credit-cards": function(e) {
                    return "Cartes de cr??dit"
                },
                "category-generated-password": function(e) {
                    return "Mot de passe g??n??r??"
                },
                "category-hide-on-this-page": function(e) {
                    return "Masquer sur cette page"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "Veuillez d??verrouiller 1Password depuis l'ic??ne dans la barre d'outils."
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return "Appuyez sur " + e.shortcut + " pour d??verrouiller 1Password"
                },
                "locked-request-unlock": function(e) {
                    return "Ouvrir 1Password"
                },
                "notification-add-account": function(e) {
                    return "Ajouter le compte ??"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "Vous pouvez ajouter et supprimer des comptes plus tard depuis 1Password"
                },
                "notification-settings": function(e) {
                    return "Param??tres"
                },
                "notification-add-account-never": function(e) {
                    return "Jamais"
                },
                "notification-add-account-confirm": function(e) {
                    return "Ajouter"
                },
                "authorize-fill": function(e) {
                    return "Cliquez sur OK pour remplir votre ??l??ment 1Password sur " + e.host
                },
                "authorize-fill-privacy": function(e) {
                    return "Cliquez sur OK pour cr??er et remplir une carte Privacy en utilisant 1Password sur " + e.host
                },
                Title: function(e) {
                    return "Titre"
                },
                "Save-new-Login": function(e) {
                    return "Enregistrer un nouvel Identifiant"
                },
                cancel: function(e) {
                    return "Annuler"
                },
                close: function(e) {
                    return "Fermer"
                },
                confirm: function(e) {
                    return "Confirmer"
                },
                Save: function(e) {
                    return "Enregistrer"
                },
                Update: function(e) {
                    return "Mise ?? jour"
                },
                "unknown-item": function(e) {
                    return "??l??ment inconnu"
                },
                "save-in": function(e) {
                    return "enregistrer dans"
                },
                "select-a-vault": function(e) {
                    return "S??lectionnez une coffre"
                },
                "locked-title": function(e) {
                    return "1Password est verrouill??"
                },
                "locked-message": function(e) {
                    return "Pour continuer ?? ??conomiser avec 1Password, d??verrouillez un compte."
                },
                "current-item": function(e) {
                    return "??l??ment actuel"
                },
                "updated-item": function(e) {
                    return "??l??ment mis ?? jour"
                },
                "add-tag": function(e) {
                    return "Ajouter un tag"
                },
                "remove-tag": function(e) {
                    return "Supprimer le tag"
                },
                "enable-privacy-header": function(e) {
                    return "Ajouter ?? 1Password"
                },
                "enable-privacy-description": function(e) {
                    return "Utilisez 1Password pour cr??er et remplir des cartes Privacy partout o?? vous payez en ligne, et enregistrez les cartes des commer??ants pour une utilisation ult??rieure."
                },
                "enable-privacy-choose-account": function(e) {
                    return "Choisir un compte"
                },
                "enable-privacy-add-button": function(e) {
                    return "Ajouter ?? 1Password"
                },
                "enable-privacy-error": function(e) {
                    return "Impossible d'activer l'int??gration Privacy. Veuillez r??essayer plus tard."
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "Veuillez d??verrouiller le compte s??lectionn?? pour activer l'int??gration."
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "Veuillez d??verrouiller 1Password pour activer l'int??gration."
                },
                "privacy-error-enable-header": function(e) {
                    return "Impossible d'activer l'int??gration"
                },
                "privacy-error-header": function(e) {
                    return "Une erreur s'est produite"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "Un abonnement 1Password est n??cessaire pour int??grer avec Privacy."
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "Veuillez vous assurer de disposer au moins d'une source de financement associ??e ?? votre compte Privacy.com, puis r??essayez."
                },
                "privacy-error-unexpected-error": function(e) {
                    return "Une erreur inattendue s'est produite. Veuillez contacter support@1password.com"
                },
                "privacy-error-okay-button": function(e) {
                    return "OK"
                },
                "privacy-once": function(e) {
                    return "Une fois"
                },
                "privacy-monthly": function(e) {
                    return "Mensuellement"
                },
                "privacy-annually": function(e) {
                    return "Annuellement"
                },
                "privacy-transaction": function(e) {
                    return "Transaction"
                },
                "privacy-forever": function(e) {
                    return "Pour toujours"
                },
                "privacy-header": function(e) {
                    return "Cr??er une carte Privacy"
                },
                "privacy-card-name-label": function(e) {
                    return "Nom de la carte"
                },
                "privacy-spend-limit-label": function(e) {
                    return "D??finir un plafond de d??penses"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "Fr??quence"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "Montant"
                },
                "privacy-funding-account-label": function(e) {
                    return "Source de financement"
                },
                "privacy-save-label": function(e) {
                    return "Enregistrer dans 1Password"
                },
                "privacy-create-button": function(e) {
                    return "Cr??er et remplir"
                },
                "currency-input-aria-label": function(e) {
                    return "Saisissez le montant en dollars"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "Veuillez activer l'int??gration Privacy dans le menu contextuel des outils du d??veloppeur."
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password est verrouill??. Veuillez r??essayer apr??s l'avoir d??bloqu??."
                },
                "privacy-error-vault-locked": function(e) {
                    return "Ce coffre est verrouill??. Veuillez r??essayer apr??s l'avoir d??bloqu??."
                },
                "privacy-error-save-failed": function(e) {
                    return "Impossible d'enregistrer cet ??l??ment. V??rifiez que le coffre est d??verrouill??, puis r??essayez."
                },
                "privacy-error-empty-card-name": function(e) {
                    return "Veuillez saisir un nom pour la carte."
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "Veuillez indiquer un nom plus court pour la carte."
                },
                "privacy-error-empty-limit": function(e) {
                    return "Vous devez indiquer une limite de d??penses pour la carte."
                },
                "privacy-error-limit-too-large": function(e) {
                    return "Veuillez indiquer une limite de d??penses inf??rieure pour la carte."
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "Impossible d'analyser l'URL"
                },
                "privacy-error-http-error": function(e) {
                    return "Nous n'avons pas pu atteindre Privacy. Veuillez v??rifier votre connexion Internet, puis r??essayez."
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "Impossible d'authentifier avec Privacy. Veuillez v??rifier votre cl?? API, puis r??essayez."
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "Erreur lors de la cr??ation de la carte??: " + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "Erreur lors de la cr??ation de la carte??."
                },
                "privacy-error-brain-error": function(e) {
                    return "Impossible de cr??er un ??l??ment de carte de cr??dit ?? partir de la carte Privacy."
                },
                "privacy-error-default": function(e) {
                    return "Une erreur inattendue s???est produite."
                },
                "notification-save-in": function(e) {
                    return "Enregistrer dans"
                },
                "notification-save-secondary": function(e) {
                    return "Enregistrer votre identifiant " + e.host + " sur 1Password???"
                },
                "notification-save-save": function(e) {
                    return "Enregistrer"
                }
            }
        },
        it: {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "Sito web sconosciuto"
                },
                "auth-type-code-to-fill": function(e) {
                    return "Digita " + e.code + " per autorizzare " + e.type + " a compilare " + e.website + "."
                },
                "auth-filling-on-website": function(e) {
                    return " su " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "Codice inserito non corretto"
                },
                "auth-why-is-this-needed": function(e) {
                    return "Perch?? questo ?? necessario?"
                },
                "list-no-items": function(e) {
                    return "Nessun elemento da mostrare."
                },
                "item-save-in-1password": function(e) {
                    return "Salva in 1Password"
                },
                "item-use-suggested-password": function(e) {
                    return "Usa password suggerita"
                },
                "item-create-a-privacy-card": function(e) {
                    return "Crea una carta Privacy"
                },
                "item-type-credit-card": function(e) {
                    return "carta di credito"
                },
                "item-type-identity": function(e) {
                    return "identit??"
                },
                "item-type-unspecified": function(e) {
                    return "elemento"
                },
                categories: function(e) {
                    return "Categorie"
                },
                "category-suggestions": function(e) {
                    return "Suggerimenti"
                },
                "category-logins": function(e) {
                    return "Dati di accesso"
                },
                "category-identities": function(e) {
                    return "Identit??"
                },
                "category-credit-cards": function(e) {
                    return "Carte di credito"
                },
                "category-generated-password": function(e) {
                    return "Password generata"
                },
                "category-hide-on-this-page": function(e) {
                    return "Nascondi su questa pagina"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "Sblocca 1Password dall'icona nella barra degli strumenti."
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return "Premi " + e.shortcut + " per sbloccare 1Password"
                },
                "locked-request-unlock": function(e) {
                    return "Apri 1Password"
                },
                "notification-add-account": function(e) {
                    return "Aggiungi account a"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "Puoi aggiungere e rimuovere account in un secondo momento da 1Password"
                },
                "notification-settings": function(e) {
                    return "Impostazioni"
                },
                "notification-add-account-never": function(e) {
                    return "Mai"
                },
                "notification-add-account-confirm": function(e) {
                    return "Aggiungi"
                },
                "authorize-fill": function(e) {
                    return "Clicca OK per compilare il tuo elemento di 1Password su " + e.host
                },
                "authorize-fill-privacy": function(e) {
                    return "Clicca su OK per creare e compilare una carta Privacy utilizzando 1Password su " + e.host
                },
                Title: function(e) {
                    return "Titolo"
                },
                "Save-new-Login": function(e) {
                    return "Salva nuovo accesso"
                },
                cancel: function(e) {
                    return "Annulla"
                },
                close: function(e) {
                    return "Chiudi"
                },
                confirm: function(e) {
                    return "Conferma"
                },
                Save: function(e) {
                    return "Salva"
                },
                Update: function(e) {
                    return "Aggiorna"
                },
                "unknown-item": function(e) {
                    return "elemento sconosciuto"
                },
                "save-in": function(e) {
                    return "salva in"
                },
                "select-a-vault": function(e) {
                    return "Seleziona una cassaforte"
                },
                "locked-title": function(e) {
                    return "1Password ?? bloccato"
                },
                "locked-message": function(e) {
                    return "Per continuare a salvare con 1Password, sblocca un account."
                },
                "current-item": function(e) {
                    return "Elemento attuale"
                },
                "updated-item": function(e) {
                    return "Elemento aggiornato"
                },
                "add-tag": function(e) {
                    return "Aggiungi etichetta"
                },
                "remove-tag": function(e) {
                    return "Rimuovi etichetta"
                },
                "enable-privacy-header": function(e) {
                    return "Aggiungi a 1Password"
                },
                "enable-privacy-description": function(e) {
                    return "Usa 1Password per creare e compilare le carte Privacy ovunque paghi online, e salva le carte del commerciante per gli utilizzi futuri."
                },
                "enable-privacy-choose-account": function(e) {
                    return "Scegli un account"
                },
                "enable-privacy-add-button": function(e) {
                    return "Aggiungi a 1Password"
                },
                "enable-privacy-error": function(e) {
                    return "Impossibile abilitare l'integrazione di Privacy. Per favore riprova pi?? tardi."
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "Sblocca l'account selezionato per abilitare l'integrazione."
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "Sblocca 1Password per abilitare l'integrazione."
                },
                "privacy-error-enable-header": function(e) {
                    return "Impossibile abilitare l'integrazione"
                },
                "privacy-error-header": function(e) {
                    return "Si ?? verificato un errore"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "?? necessario l'abbonamento a 1Password per l'integrazione con Privacy."
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "Assicurati che ci sia almeno una fonte di finanziamento associata al tuo conto su Privacy.com, quindi riprova."
                },
                "privacy-error-unexpected-error": function(e) {
                    return "Si ?? verificato un errore imprevisto. Contatta support@1password.com"
                },
                "privacy-error-okay-button": function(e) {
                    return "OK"
                },
                "privacy-once": function(e) {
                    return "Una volta"
                },
                "privacy-monthly": function(e) {
                    return "Mensilmente"
                },
                "privacy-annually": function(e) {
                    return "Annualmente"
                },
                "privacy-transaction": function(e) {
                    return "Transazione"
                },
                "privacy-forever": function(e) {
                    return "Per sempre"
                },
                "privacy-header": function(e) {
                    return "Crea una carta Privacy"
                },
                "privacy-card-name-label": function(e) {
                    return "Nome della carta"
                },
                "privacy-spend-limit-label": function(e) {
                    return "Imposta il limite di spesa"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "Frequenza"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "Importo"
                },
                "privacy-funding-account-label": function(e) {
                    return "Fonte di finanziamento"
                },
                "privacy-save-label": function(e) {
                    return "Salva in 1Password"
                },
                "privacy-create-button": function(e) {
                    return "Crea e compila"
                },
                "currency-input-aria-label": function(e) {
                    return "Inserisci l'importo in dollari"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "Abilita l'integrazione di Privacy dal menu contestuale Strumenti per sviluppatori."
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password ?? bloccato. Prova di nuovo dopo lo sblocco."
                },
                "privacy-error-vault-locked": function(e) {
                    return "Questa cassaforte ?? bloccata. Prova di nuovo dopo lo sblocco."
                },
                "privacy-error-save-failed": function(e) {
                    return "Impossibile salvare l'elemento. Verifica che la cassaforte sia sbloccata e riprova."
                },
                "privacy-error-empty-card-name": function(e) {
                    return "Inserisci un nome per la carta."
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "Inserisci un nome pi?? breve per la carta."
                },
                "privacy-error-empty-limit": function(e) {
                    return "?? necessario fornire un limite di spesa per la carta."
                },
                "privacy-error-limit-too-large": function(e) {
                    return "Inserisci un limite di spesa inferiore per la carta."
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "Impossibile analizzare l'URL"
                },
                "privacy-error-http-error": function(e) {
                    return "Impossibile raggiungere Privacy. Controlla la connessione a internet e riprova."
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "Impossibile eseguire l'autenticazione con Privacy. Controlla la tua chiave API e riprova."
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "Errore durante la creazione della carta: " + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "Errore durante la creazione della carta."
                },
                "privacy-error-brain-error": function(e) {
                    return "Impossibile creare l'elemento della carta di credito dalla carta Privacy."
                },
                "privacy-error-default": function(e) {
                    return "Si ?? verificato un errore imprevisto."
                },
                "notification-save-in": function(e) {
                    return "Salva in"
                },
                "notification-save-secondary": function(e) {
                    return "Vuoi salvare le tue credenziali di " + e.host + " su 1Password?"
                },
                "notification-save-save": function(e) {
                    return "Salva"
                }
            }
        },
        ja: {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "???????????????????????????"
                },
                "auth-type-code-to-fill": function(e) {
                    return e.code + " ???????????????" + e.website + " ???" + e.type + " ?????????????????????"
                },
                "auth-filling-on-website": function(e) {
                    return " ????????? " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "??????????????????????????????????????????"
                },
                "auth-why-is-this-needed": function(e) {
                    return "?????????????????????????????????"
                },
                "list-no-items": function(e) {
                    return "???????????????????????????????????????"
                },
                "item-save-in-1password": function(e) {
                    return "1Password???????????????"
                },
                "item-use-suggested-password": function(e) {
                    return "?????????????????????????????????????????????"
                },
                "item-create-a-privacy-card": function(e) {
                    return "????????????????????????????????????"
                },
                "item-type-credit-card": function(e) {
                    return "????????????????????????"
                },
                "item-type-identity": function(e) {
                    return "????????????"
                },
                "item-type-unspecified": function(e) {
                    return "????????????"
                },
                categories: function(e) {
                    return "????????????"
                },
                "category-suggestions": function(e) {
                    return "????????????"
                },
                "category-logins": function(e) {
                    return "????????????"
                },
                "category-identities": function(e) {
                    return "????????????"
                },
                "category-credit-cards": function(e) {
                    return "????????????????????????"
                },
                "category-generated-password": function(e) {
                    return "??????????????????????????????"
                },
                "category-hide-on-this-page": function(e) {
                    return "?????????????????????????????????"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "????????????????????????????????????1Password???????????????????????????????????????"
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return e.shortcut + " ??????????????????1Password??????????????????"
                },
                "locked-request-unlock": function(e) {
                    return "1Password?????????"
                },
                "notification-add-account": function(e) {
                    return "???????????????????????????"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "???????????????????????????1Password??????????????????????????????????????????????????????"
                },
                "notification-settings": function(e) {
                    return "??????"
                },
                "notification-add-account-never": function(e) {
                    return "???????????????"
                },
                "notification-add-account-confirm": function(e) {
                    return "????????????"
                },
                "authorize-fill": function(e) {
                    return "???OK???????????????????????????" + e.host + " ???1Password??????????????????????????????"
                },
                "authorize-fill-privacy": function(e) {
                    return "OK???????????????????????????????????????????????????????????????" + e.host + " ??????1Password?????????????????????????????????"
                },
                Title: function(e) {
                    return "????????????"
                },
                "Save-new-Login": function(e) {
                    return "???????????????????????????"
                },
                cancel: function(e) {
                    return "???????????????"
                },
                close: function(e) {
                    return "?????????"
                },
                confirm: function(e) {
                    return "??????"
                },
                Save: function(e) {
                    return "??????"
                },
                Update: function(e) {
                    return "??????????????????"
                },
                "unknown-item": function(e) {
                    return "?????????????????????"
                },
                "save-in": function(e) {
                    return "??????"
                },
                "select-a-vault": function(e) {
                    return "??????????????????"
                },
                "locked-title": function(e) {
                    return "1Password ??????????????????????????????"
                },
                "locked-message": function(e) {
                    return "????????????1Password?????????????????????????????????????????????????????????????????????????????????"
                },
                "current-item": function(e) {
                    return "?????????????????????"
                },
                "updated-item": function(e) {
                    return "???????????????????????????"
                },
                "add-tag": function(e) {
                    return "???????????????"
                },
                "remove-tag": function(e) {
                    return "???????????????"
                },
                "enable-privacy-header": function(e) {
                    return "1Password?????????"
                },
                "enable-privacy-description": function(e) {
                    return "1Password?????????????????????????????????????????????????????????????????????Privacy Cards?????????????????????????????????????????????????????????????????????????????????????????????"
                },
                "enable-privacy-choose-account": function(e) {
                    return "????????????????????????"
                },
                "enable-privacy-add-button": function(e) {
                    return "1Password ?????????"
                },
                "enable-privacy-error": function(e) {
                    return "Privacy????????????????????????????????????????????????????????????????????????????????????????????????"
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "???????????????????????????????????????????????????????????????????????????????????????????????????"
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "?????????????????????????????????1Password ???????????????????????????????????????"
                },
                "privacy-error-enable-header": function(e) {
                    return "?????????????????????????????????"
                },
                "privacy-error-header": function(e) {
                    return "??????????????????????????????"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "??????????????????????????????????????????1Password???????????????????????????????????????"
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "Privacy.com????????????????????????????????????1?????????????????????????????????????????????????????????????????????????????????????????????"
                },
                "privacy-error-unexpected-error": function(e) {
                    return "????????????????????????????????????????????????support@1password.com ??????????????????????????????"
                },
                "privacy-error-okay-button": function(e) {
                    return "OK"
                },
                "privacy-once": function(e) {
                    return "1???"
                },
                "privacy-monthly": function(e) {
                    return "??????"
                },
                "privacy-annually": function(e) {
                    return "??????"
                },
                "privacy-transaction": function(e) {
                    return "??????"
                },
                "privacy-forever": function(e) {
                    return "??????"
                },
                "privacy-header": function(e) {
                    return "Privacy Card?????????"
                },
                "privacy-card-name-label": function(e) {
                    return "???????????????"
                },
                "privacy-spend-limit-label": function(e) {
                    return "????????????????????????"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "??????"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "??????"
                },
                "privacy-funding-account-label": function(e) {
                    return "???????????????"
                },
                "privacy-save-label": function(e) {
                    return "1Password???????????????"
                },
                "privacy-create-button": function(e) {
                    return "???????????????"
                },
                "currency-input-aria-label": function(e) {
                    return "??????????????????????????????????????????"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "?????????????????????????????????????????????????????????Privacy??????????????????????????????????????????"
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password???????????????????????????????????????????????????????????????????????????????????????????????????"
                },
                "privacy-error-vault-locked": function(e) {
                    return "????????????????????????????????????????????????????????????????????????????????????????????????????????????"
                },
                "privacy-error-save-failed": function(e) {
                    return "????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????"
                },
                "privacy-error-empty-card-name": function(e) {
                    return "????????????????????????????????????????????????"
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "??????????????????????????????????????????????????????"
                },
                "privacy-error-empty-limit": function(e) {
                    return "?????????????????????????????????????????????????????????"
                },
                "privacy-error-limit-too-large": function(e) {
                    return "??????????????????????????????????????????????????????????????????"
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "URL???????????????????????????"
                },
                "privacy-error-http-error": function(e) {
                    return "Privacy??????????????????????????????????????????????????????????????????????????????????????????????????????????????????"
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "Privacy??????????????????????????????API?????????????????????????????????????????????????????????"
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "???????????????????????????" + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "???????????????????????????"
                },
                "privacy-error-brain-error": function(e) {
                    return "Privacy???????????????????????????????????????????????????????????????????????????"
                },
                "privacy-error-default": function(e) {
                    return "????????????????????????????????????????????????"
                },
                "notification-save-in": function(e) {
                    return "????????????:"
                },
                "notification-save-secondary": function(e) {
                    return e.host + " ??????????????? 1Password ??????????????????????"
                },
                "notification-save-save": function(e) {
                    return "??????"
                }
            }
        },
        ko: {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "???????????? ?????? ????????????"
                },
                "auth-type-code-to-fill": function(e) {
                    return e.website + " " + e.type + "??? ?????? ????????? ??????????????? " + e.code + "???(???) ???????????????."
                },
                "auth-filling-on-website": function(e) {
                    return e.website + "??????"
                },
                "auth-incorrect-code-entered": function(e) {
                    return "????????? ????????? ?????????????????????"
                },
                "auth-why-is-this-needed": function(e) {
                    return "????????? ??? ????????????????"
                },
                "list-no-items": function(e) {
                    return "????????? ????????? ????????????."
                },
                "item-save-in-1password": function(e) {
                    return "1Password??? ??????"
                },
                "item-use-suggested-password": function(e) {
                    return "????????? ???????????? ??????"
                },
                "item-create-a-privacy-card": function(e) {
                    return "Privacy ?????? ??????"
                },
                "item-type-credit-card": function(e) {
                    return "????????????"
                },
                "item-type-identity": function(e) {
                    return "?????? ??????"
                },
                "item-type-unspecified": function(e) {
                    return "??????"
                },
                categories: function(e) {
                    return "????????????"
                },
                "category-suggestions": function(e) {
                    return "??????"
                },
                "category-logins": function(e) {
                    return "????????? ??????"
                },
                "category-identities": function(e) {
                    return "?????? ??????"
                },
                "category-credit-cards": function(e) {
                    return "????????????"
                },
                "category-generated-password": function(e) {
                    return "????????? ????????????"
                },
                "category-hide-on-this-page": function(e) {
                    return "??? ??????????????? ?????????"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "?????? ?????? ??????????????? 1Password??? ?????? ???????????????."
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return e.shortcut + "???(???) ????????? 1Password??? ?????? ???????????????"
                },
                "locked-request-unlock": function(e) {
                    return "1Password ??????"
                },
                "notification-add-account": function(e) {
                    return "????????? ?????? ??????"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "????????? 1Password?????? ????????? ?????? ?????? ????????? ??? ????????????"
                },
                "notification-settings": function(e) {
                    return "??????"
                },
                "notification-add-account-never": function(e) {
                    return "??? ???"
                },
                "notification-add-account-confirm": function(e) {
                    return "??????"
                },
                "authorize-fill": function(e) {
                    return e.host + "?????? 1Password ????????? ???????????? ????????? ???????????????"
                },
                "authorize-fill-privacy": function(e) {
                    return "'??????'??? ???????????? Privacy ????????? ????????????, " + e.host + "?????? 1Password??? ???????????? ???????????? ???????????????"
                },
                Title: function(e) {
                    return "??????"
                },
                "Save-new-Login": function(e) {
                    return "??? ????????? ?????? ??????"
                },
                cancel: function(e) {
                    return "??????"
                },
                close: function(e) {
                    return "??????"
                },
                confirm: function(e) {
                    return "??????"
                },
                Save: function(e) {
                    return "??????"
                },
                Update: function(e) {
                    return "????????????"
                },
                "unknown-item": function(e) {
                    return "??? ??? ?????? ??????"
                },
                "save-in": function(e) {
                    return "?????? ??????"
                },
                "select-a-vault": function(e) {
                    return "?????? ??????"
                },
                "locked-title": function(e) {
                    return "1Password??? ?????? ????????????"
                },
                "locked-message": function(e) {
                    return "?????? 1Password??? ????????? ??????????????? ????????? ?????? ???????????????."
                },
                "current-item": function(e) {
                    return "?????? ??????"
                },
                "updated-item": function(e) {
                    return "??????????????? ??????"
                },
                "add-tag": function(e) {
                    return "?????? ??????"
                },
                "remove-tag": function(e) {
                    return "?????? ??????"
                },
                "enable-privacy-header": function(e) {
                    return "1Password??? ??????"
                },
                "enable-privacy-description": function(e) {
                    return "????????? ????????? ????????? ?????? ???????????? 1Password??? ????????? Privacy ????????? ?????? ??? ????????????, ?????? ????????? ?????? ????????? ????????? ????????? ????????? ?????????."
                },
                "enable-privacy-choose-account": function(e) {
                    return "?????? ??????"
                },
                "enable-privacy-add-button": function(e) {
                    return "1Password??? ??????"
                },
                "enable-privacy-error": function(e) {
                    return "Privacy ????????? ???????????? ??? ????????????. ????????? ?????? ???????????????."
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "????????? ?????????????????? ????????? ????????? ?????? ???????????????."
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "????????? ?????????????????? 1Password??? ?????? ???????????????."
                },
                "privacy-error-enable-header": function(e) {
                    return "????????? ???????????? ??? ????????????"
                },
                "privacy-error-header": function(e) {
                    return "????????? ??????????????????"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "Privacy??? ??????????????? 1Password ???????????? ???????????????."
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "???????????? Privacy.com ????????? ?????? ????????? ???????????? ???????????? ????????? ????????? ???, ?????? ???????????????."
                },
                "privacy-error-unexpected-error": function(e) {
                    return "????????? ?????? ????????? ??????????????????. support@1password.com?????? ????????? ?????????"
                },
                "privacy-error-okay-button": function(e) {
                    return "??????"
                },
                "privacy-once": function(e) {
                    return "??? ???"
                },
                "privacy-monthly": function(e) {
                    return "??????"
                },
                "privacy-annually": function(e) {
                    return "??????"
                },
                "privacy-transaction": function(e) {
                    return "??????"
                },
                "privacy-forever": function(e) {
                    return "?????????"
                },
                "privacy-header": function(e) {
                    return "Privacy ?????? ??????"
                },
                "privacy-card-name-label": function(e) {
                    return "?????? ??????"
                },
                "privacy-spend-limit-label": function(e) {
                    return "?????? ?????? ??????"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "??????"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "??????"
                },
                "privacy-funding-account-label": function(e) {
                    return "?????? ??????"
                },
                "privacy-save-label": function(e) {
                    return "1Password??? ??????"
                },
                "privacy-create-button": function(e) {
                    return "?????? ??? ?????? ??????"
                },
                "currency-input-aria-label": function(e) {
                    return "????????? ????????? ???????????????"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "????????? ?????? ???????????? ???????????? Privacy ????????? ??????????????????."
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password??? ?????? ????????????. ????????? ????????? ??? ?????? ???????????????."
                },
                "privacy-error-vault-locked": function(e) {
                    return "??? ????????? ?????? ????????????. ????????? ????????? ??? ?????? ???????????????."
                },
                "privacy-error-save-failed": function(e) {
                    return "????????? ????????? ??? ????????????. ????????? ?????? ????????? ????????? ??? ?????? ???????????????."
                },
                "privacy-error-empty-card-name": function(e) {
                    return "????????? ????????? ???????????????."
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "??? ?????? ?????? ????????? ???????????????."
                },
                "privacy-error-empty-limit": function(e) {
                    return "????????? ?????? ????????? ??????????????? ?????????."
                },
                "privacy-error-limit-too-large": function(e) {
                    return "??? ?????? ?????? ?????? ????????? ???????????????."
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "URL??? ?????? ????????? ??? ????????????"
                },
                "privacy-error-http-error": function(e) {
                    return "Privacy??? ???????????? ???????????????. ????????? ?????? ????????? ????????? ??? ?????? ???????????????."
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "Privacy ????????? ????????? ??? ????????????. API ?????? ????????? ??? ?????? ???????????????."
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "?????? ?????? ??? ?????? ??????: " + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "?????? ?????? ??? ?????? ??????."
                },
                "privacy-error-brain-error": function(e) {
                    return "Privacy ???????????? ???????????? ????????? ????????? ??? ????????????."
                },
                "privacy-error-default": function(e) {
                    return "????????? ?????? ????????? ??????????????????."
                },
                "notification-save-in": function(e) {
                    return "?????? ??????"
                },
                "notification-save-secondary": function(e) {
                    return "1Password??? " + e.host + " ????????? ????????? ????????????????"
                },
                "notification-save-save": function(e) {
                    return "??????"
                }
            }
        },
        pt: {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "Site desconhecido"
                },
                "auth-type-code-to-fill": function(e) {
                    return "Digite " + e.code + " para autorizar o preenchimento de " + e.type + " em " + e.website + "."
                },
                "auth-filling-on-website": function(e) {
                    return " em " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "Foi informado um c??digo incorreto"
                },
                "auth-why-is-this-needed": function(e) {
                    return "Por que isso ?? necess??rio?"
                },
                "list-no-items": function(e) {
                    return "Nenhum item para mostrar."
                },
                "item-save-in-1password": function(e) {
                    return "Salvar no 1Password"
                },
                "item-use-suggested-password": function(e) {
                    return "Usar a senha sugerida"
                },
                "item-create-a-privacy-card": function(e) {
                    return "Criar um cart??o Privacy"
                },
                "item-type-credit-card": function(e) {
                    return "cart??o de cr??dito"
                },
                "item-type-identity": function(e) {
                    return "identidade"
                },
                "item-type-unspecified": function(e) {
                    return "item"
                },
                categories: function(e) {
                    return "Categorias"
                },
                "category-suggestions": function(e) {
                    return "Sugest??es"
                },
                "category-logins": function(e) {
                    return "Credenciais"
                },
                "category-identities": function(e) {
                    return "Identidades"
                },
                "category-credit-cards": function(e) {
                    return "Cart??es de cr??dito"
                },
                "category-generated-password": function(e) {
                    return "Senha gerada"
                },
                "category-hide-on-this-page": function(e) {
                    return "Ocultar nesta p??gina"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "Desbloqueie o 1Password no ??cone da barra de ferramentas."
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return "Pressione " + e.shortcut + " para desbloquear o 1Password"
                },
                "locked-request-unlock": function(e) {
                    return "Abrir o 1Password"
                },
                "notification-add-account": function(e) {
                    return "Adicionar conta ao"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "Voc?? poder?? adicionar e remover contas do 1Password depois"
                },
                "notification-settings": function(e) {
                    return "Configura????es"
                },
                "notification-add-account-never": function(e) {
                    return "Nunca"
                },
                "notification-add-account-confirm": function(e) {
                    return "Adicionar"
                },
                "authorize-fill": function(e) {
                    return "Clique em OK para preencher o item do 1Password no " + e.host
                },
                "authorize-fill-privacy": function(e) {
                    return "Clique em OK para criar e preencher um cart??o Privacy com o1Password em " + e.host
                },
                Title: function(e) {
                    return "T??tulo"
                },
                "Save-new-Login": function(e) {
                    return "Salvar novas credenciais"
                },
                cancel: function(e) {
                    return "Cancelar"
                },
                close: function(e) {
                    return "Fechar"
                },
                confirm: function(e) {
                    return "Confirmar"
                },
                Save: function(e) {
                    return "Salvar"
                },
                Update: function(e) {
                    return "Atualizar"
                },
                "unknown-item": function(e) {
                    return "item desconhecido"
                },
                "save-in": function(e) {
                    return "salvar em"
                },
                "select-a-vault": function(e) {
                    return "Selecione um cofre"
                },
                "locked-title": function(e) {
                    return "O 1Password est?? bloqueado"
                },
                "locked-message": function(e) {
                    return "Para continuar salvando com o 1Password, desbloqueie uma conta."
                },
                "current-item": function(e) {
                    return "Item atual"
                },
                "updated-item": function(e) {
                    return "Item atualizado"
                },
                "add-tag": function(e) {
                    return "Adicionar etiqueta"
                },
                "remove-tag": function(e) {
                    return "Remover etiqueta"
                },
                "enable-privacy-header": function(e) {
                    return "Adicionar ao 1Password"
                },
                "enable-privacy-description": function(e) {
                    return "Use o 1Password para criar e preencher cart??es do Privacy em todos os lugares onde voc?? pagar online e guardar cart??es de vendedores para usar futuramente."
                },
                "enable-privacy-choose-account": function(e) {
                    return "Escolha uma conta"
                },
                "enable-privacy-add-button": function(e) {
                    return "Adicionar ao 1Password"
                },
                "enable-privacy-error": function(e) {
                    return "N??o foi poss??vel ativar a integra????o com o Privacy. Tente novamente mais tarde."
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "Desbloqueie a conta selecionada para ativar a integra????o."
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "Desbloqueie o 1Password para ativar a integra????o."
                },
                "privacy-error-enable-header": function(e) {
                    return "N??o foi poss??vel ativar a integra????o"
                },
                "privacy-error-header": function(e) {
                    return "Ocorreu um erro"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "?? necess??rio uma associa????o do 1Password para integrar com o Privacy."
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "Verifique se h?? pelo menos uma Origem de Financiamento associada ?? sua conta no Privacy.com e tente novamente."
                },
                "privacy-error-unexpected-error": function(e) {
                    return "Ocorreu um erro inesperado. Entre em contato com support@1password.com"
                },
                "privacy-error-okay-button": function(e) {
                    return "OK"
                },
                "privacy-once": function(e) {
                    return "Uma vez"
                },
                "privacy-monthly": function(e) {
                    return "Mensal"
                },
                "privacy-annually": function(e) {
                    return "Anual"
                },
                "privacy-transaction": function(e) {
                    return "Transa????o"
                },
                "privacy-forever": function(e) {
                    return "Vital??cio"
                },
                "privacy-header": function(e) {
                    return "Criar um cart??o do Privacy"
                },
                "privacy-card-name-label": function(e) {
                    return "Nome do cart??o"
                },
                "privacy-spend-limit-label": function(e) {
                    return "Definir o limite de gastos"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "Frequ??ncia"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "Valor"
                },
                "privacy-funding-account-label": function(e) {
                    return "Origem do financiamento"
                },
                "privacy-save-label": function(e) {
                    return "Salvar no 1Password"
                },
                "privacy-create-button": function(e) {
                    return "Criar e preencher"
                },
                "currency-input-aria-label": function(e) {
                    return "Digite o valor em d??lares"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "Ative a integra????o com o Privacy no menu de contexto Ferramentas de desenvolvedor."
                },
                "privacy-error-app-locked": function(e) {
                    return "O 1Password est?? bloqueado. Tente novamente ap??s desbloquear."
                },
                "privacy-error-vault-locked": function(e) {
                    return "Esse cofre est?? bloqueado. Desbloqueie e tente novamente."
                },
                "privacy-error-save-failed": function(e) {
                    return "N??o foi poss??vel salvar o item. Verifique se o cofre est?? trancado e tente novamente."
                },
                "privacy-error-empty-card-name": function(e) {
                    return "Digite um nome para o cart??o."
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "Digite um nome menor para o cart??o."
                },
                "privacy-error-empty-limit": function(e) {
                    return "Voc?? precisa informar um limite de gastos para o cart??o."
                },
                "privacy-error-limit-too-large": function(e) {
                    return "Digite um limite de gastos menor para o cart??o."
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "N??o foi poss??vel processar o URL"
                },
                "privacy-error-http-error": function(e) {
                    return "N??o foi poss??vel acessar o Privacy. Verifique a conex??o com a internet e tente novamente."
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "N??o foi poss??vel autenticar na Privacy. Confira a chave da API e tente novamente."
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "Erro na cria????o do cart??o: " + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "Erro na cria????o do cart??o."
                },
                "privacy-error-brain-error": function(e) {
                    return "N??o foi poss??vel criar o item de cart??o de cr??dito do cart??o Privacy."
                },
                "privacy-error-default": function(e) {
                    return "Ocorreu um erro inesperado."
                },
                "notification-save-in": function(e) {
                    return "Salvar em"
                },
                "notification-save-secondary": function(e) {
                    return "Salvar sua credencial de " + e.host + " no 1Password?"
                },
                "notification-save-save": function(e) {
                    return "Salvar"
                }
            }
        },
        ru: {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "?????????????????????? ??????-????????"
                },
                "auth-type-code-to-fill": function(e) {
                    return "?????????????? " + e.code + " ?????? ?????????????????????? " + e.type + " ???????????????????? " + e.website + "."
                },
                "auth-filling-on-website": function(e) {
                    return " ???? " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "???????????? ???????????????? ??????"
                },
                "auth-why-is-this-needed": function(e) {
                    return "???????????? ?????? ?????????????????????"
                },
                "list-no-items": function(e) {
                    return "?????? ?????????????????? ?????? ??????????????????????."
                },
                "item-save-in-1password": function(e) {
                    return "?????????????????? ?? 1Password"
                },
                "item-use-suggested-password": function(e) {
                    return "???????????????????????? ???????????????????????? ????????????"
                },
                "item-create-a-privacy-card": function(e) {
                    return "?????????????? ?????????? Privacy"
                },
                "item-type-credit-card": function(e) {
                    return "???????????????????? ??????????"
                },
                "item-type-identity": function(e) {
                    return "????????-?? ????????????????"
                },
                "item-type-unspecified": function(e) {
                    return "??????????????"
                },
                categories: function(e) {
                    return "??????????????????"
                },
                "category-suggestions": function(e) {
                    return "??????????????????????"
                },
                "category-logins": function(e) {
                    return "???????????? ?????? ??????????"
                },
                "category-identities": function(e) {
                    return "????????-?? ????????????????"
                },
                "category-credit-cards": function(e) {
                    return "???????????????????? ??????????"
                },
                "category-generated-password": function(e) {
                    return "?????????????????????????????? ????????????"
                },
                "category-hide-on-this-page": function(e) {
                    return "???????????? ???? ???????? ????????????????"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "????????????????????, ?????????????????????????? 1Password, ?????????? ???? ???????????? ?? ???????????? ????????????????????????."
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return "?????????????? " + e.shortcut + " ?????? ?????????????????????????? 1Password"
                },
                "locked-request-unlock": function(e) {
                    return "?????????????? 1Password"
                },
                "notification-add-account": function(e) {
                    return "???????????????? ?????????????? ??"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "???? ???????????? ???????????????? ?????? ?????????????? ???????????????? ???? 1Password ??????????, ?????????????? ?? ??????????????????"
                },
                "notification-settings": function(e) {
                    return "??????????????????"
                },
                "notification-add-account-never": function(e) {
                    return "??????????????"
                },
                "notification-add-account-confirm": function(e) {
                    return "????????????????"
                },
                "authorize-fill": function(e) {
                    return "?????????????? ????, ?????????? ?????????????????? ?????????????? 1Password ???? " + e.host
                },
                "authorize-fill-privacy": function(e) {
                    return "?????????????? OK, ?????????? ?????????????? ?? ?????????????????? ???????????? ?????? ?????????? Privacy, ?????????????????? 1Password ???? " + e.host
                },
                Title: function(e) {
                    return "????????????????"
                },
                "Save-new-Login": function(e) {
                    return "?????????????????? ?????????? ??????????"
                },
                cancel: function(e) {
                    return "????????????????"
                },
                close: function(e) {
                    return "??????????????"
                },
                confirm: function(e) {
                    return "??????????????????????"
                },
                Save: function(e) {
                    return "??????????????????"
                },
                Update: function(e) {
                    return "????????????????"
                },
                "unknown-item": function(e) {
                    return "?????????????????????? ??????????????"
                },
                "save-in": function(e) {
                    return "?????????????????? ??"
                },
                "select-a-vault": function(e) {
                    return "???????????????? ????????"
                },
                "locked-title": function(e) {
                    return "1Password ????????????????????????"
                },
                "locked-message": function(e) {
                    return "?????????? ???????????????????? ?? ?????????????????? ???????????? ?? 1Password, ?????????????????????????? ??????????????."
                },
                "current-item": function(e) {
                    return "?????????????? ??????????????"
                },
                "updated-item": function(e) {
                    return "?????????????????????? ??????????????"
                },
                "add-tag": function(e) {
                    return "???????????????? ??????"
                },
                "remove-tag": function(e) {
                    return "?????????????? ??????"
                },
                "enable-privacy-header": function(e) {
                    return "???????????????? ?? 1Password"
                },
                "enable-privacy-description": function(e) {
                    return "?????????????????????? 1Password, ?????????? ?????????????????? ?? ?????????????????? ???????????? ???????? Privacy ?????? ?????????? ???????????? ???????????? ?? ?????????????????? ?????????? ?????????????????? ?????? ?????????????????????? ??????????????????????????."
                },
                "enable-privacy-choose-account": function(e) {
                    return "???????????????? ??????????????"
                },
                "enable-privacy-add-button": function(e) {
                    return "???????????????? ?? 1Password"
                },
                "enable-privacy-error": function(e) {
                    return "???? ?????????????? ???????????????? ???????????????????? ?? Privacy. ????????????????????, ?????????????????? ?????????????? ??????????."
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "????????????????????, ?????????????????????????? ?????????????????? ?????????????? ?????? ?????????????????? ????????????????????."
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "????????????????????, ?????????????????????????? 1Password ?????? ?????????????????? ????????????????????."
                },
                "privacy-error-enable-header": function(e) {
                    return "???? ?????????????? ???????????????? ????????????????????"
                },
                "privacy-error-header": function(e) {
                    return "?????????????????? ????????????"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "?????? ???????????????????? ?? Privacy ?????????????????? ???????????????? ???? 1Password."
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "????????????????????, ?????????????????? ?? ??????, ?????? ?? ???????????? ???????????????? ???? Privacy.com ???????????????????? ???? ?????????????? ???????? ???????? ???????????????? ????????????????????????????."
                },
                "privacy-error-unexpected-error": function(e) {
                    return "???????????????? ???????????????????????????? ????????????. ????????????????????, ?????????????????? ?? ???????? ???? ???????????? support@1password.com"
                },
                "privacy-error-okay-button": function(e) {
                    return "????"
                },
                "privacy-once": function(e) {
                    return "??????????????????????"
                },
                "privacy-monthly": function(e) {
                    return "????????????????????"
                },
                "privacy-annually": function(e) {
                    return "????????????????"
                },
                "privacy-transaction": function(e) {
                    return "????????????????????"
                },
                "privacy-forever": function(e) {
                    return "????????????????"
                },
                "privacy-header": function(e) {
                    return "?????????????? ?????????? Privacy"
                },
                "privacy-card-name-label": function(e) {
                    return "???????????????? ??????????"
                },
                "privacy-spend-limit-label": function(e) {
                    return "???????????????????? ?????????? ????????????????"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "??????????????"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "??????????"
                },
                "privacy-funding-account-label": function(e) {
                    return "???????????????? ????????????????????????????"
                },
                "privacy-save-label": function(e) {
                    return "?????????????????? ?? 1Password"
                },
                "privacy-create-button": function(e) {
                    return "?????????????? ?? ??????????????????"
                },
                "currency-input-aria-label": function(e) {
                    return "?????????????? ?????????? ?? ????????????????"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "????????????????????, ???????????????? ???????????????????? ?? Privacy ?? ?????????????????????? ???????? ???????????????????????? ????????????????????????."
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password ????????????????????????. ????????????????????, ???????????????????? ?????? ?????? ?????????? ??????????????????????????."
                },
                "privacy-error-vault-locked": function(e) {
                    return "???????? ???????? ????????????????????????. ????????????????????, ???????????????????? ?????? ?????? ?????????? ??????????????????????????."
                },
                "privacy-error-save-failed": function(e) {
                    return "???? ?????????????? ?????????????????? ??????????????. ??????????????????, ?????? ???????? ?????????????????????????? ?? ?????????????????? ??????????????."
                },
                "privacy-error-empty-card-name": function(e) {
                    return "????????????????????, ?????????????? ???????????????? ??????????."
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "????????????????????, ?????????????? ?????????? ???????????????? ???????????????? ??????????."
                },
                "privacy-error-empty-limit": function(e) {
                    return "?????? ?????????? ???????????????????? ???????????????????? ?????????????????????? ???? ?????????????? ????????."
                },
                "privacy-error-limit-too-large": function(e) {
                    return "????????????????????, ?????????????? ?????????? ???????????????? ???? ??????????."
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "???? ?????????????? ?????????????? URL-??????????"
                },
                "privacy-error-http-error": function(e) {
                    return "?????? ???? ?????????????? ?????????????????? ?? Privacy. ????????????????????, ?????????????????? ???????? ????????????????-???????????????????? ?? ???????????????????? ??????????."
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "???? ?????????????? ?????????????????????? ?????????????????????? ?? ?????????????? Privacy. ????????????????????, ?????????????????? ?????? API ???????? ?? ???????????????????? ?????? ??????."
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "?????? ???????????????? ?????????? ???????????????? ????????????: " + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "?????? ???????????????? ?????????? ???????????????? ????????????."
                },
                "privacy-error-brain-error": function(e) {
                    return "???? ?????????????? ?????????????? ?????????????? ?????????????????? ?????????? ?????? ?????????? Privacy."
                },
                "privacy-error-default": function(e) {
                    return "?????????????????? ???????????????????????????? ????????????."
                },
                "notification-save-in": function(e) {
                    return "?????????????????? ??"
                },
                "notification-save-secondary": function(e) {
                    return "?????????????????? ???????? ???????????? ?????? ?????????? " + e.host + " ?? 1Password?"
                },
                "notification-save-save": function(e) {
                    return "??????????????????"
                }
            }
        },
        "zh-CN": {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "??????????????????"
                },
                "auth-type-code-to-fill": function(e) {
                    return "?????? " + e.code + " ????????? " + e.type + " ??????" + e.website + "???"
                },
                "auth-filling-on-website": function(e) {
                    return "??? " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "???????????????????????????"
                },
                "auth-why-is-this-needed": function(e) {
                    return "???????????????????????????"
                },
                "list-no-items": function(e) {
                    return "???????????????????????????"
                },
                "item-save-in-1password": function(e) {
                    return "??? 1Password ?????????"
                },
                "item-use-suggested-password": function(e) {
                    return "?????????????????????"
                },
                "item-create-a-privacy-card": function(e) {
                    return "?????? Privacy ??????"
                },
                "item-type-credit-card": function(e) {
                    return "?????????"
                },
                "item-type-identity": function(e) {
                    return "????????????"
                },
                "item-type-unspecified": function(e) {
                    return "??????"
                },
                categories: function(e) {
                    return "??????"
                },
                "category-suggestions": function(e) {
                    return "??????"
                },
                "category-logins": function(e) {
                    return "????????????"
                },
                "category-identities": function(e) {
                    return "????????????"
                },
                "category-credit-cards": function(e) {
                    return "?????????"
                },
                "category-generated-password": function(e) {
                    return "??????????????????"
                },
                "category-hide-on-this-page": function(e) {
                    return "??????????????????"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "?????????????????????????????? 1Password???"
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return "?????? " + e.shortcut + " ????????? 1Password"
                },
                "locked-request-unlock": function(e) {
                    return "?????? 1Password"
                },
                "notification-add-account": function(e) {
                    return "??????????????????"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "?????????????????? 1Password ????????????????????????"
                },
                "notification-settings": function(e) {
                    return "??????"
                },
                "notification-add-account-never": function(e) {
                    return "??????"
                },
                "notification-add-account-confirm": function(e) {
                    return "??????"
                },
                "authorize-fill": function(e) {
                    return "?????? OK ?????? " + e.host + " ????????? 1Password ?????????"
                },
                "authorize-fill-privacy": function(e) {
                    return "?????? OK ?????? Privacy ?????????????????? 1Password ??? " + e.host + " ??????"
                },
                Title: function(e) {
                    return "??????"
                },
                "Save-new-Login": function(e) {
                    return "?????????????????????"
                },
                cancel: function(e) {
                    return "??????"
                },
                close: function(e) {
                    return "??????"
                },
                confirm: function(e) {
                    return "??????"
                },
                Save: function(e) {
                    return "??????"
                },
                Update: function(e) {
                    return "??????"
                },
                "unknown-item": function(e) {
                    return "????????????"
                },
                "save-in": function(e) {
                    return "?????????"
                },
                "select-a-vault": function(e) {
                    return "???????????????"
                },
                "locked-title": function(e) {
                    return "1Password ?????????"
                },
                "locked-message": function(e) {
                    return "??????????????? 1Password ?????????????????????????????????"
                },
                "current-item": function(e) {
                    return "????????????"
                },
                "updated-item": function(e) {
                    return "??????????????????"
                },
                "add-tag": function(e) {
                    return "????????????"
                },
                "remove-tag": function(e) {
                    return "????????????"
                },
                "enable-privacy-header": function(e) {
                    return "????????? 1Password"
                },
                "enable-privacy-description": function(e) {
                    return "?????? 1Password ????????????????????????????????? Privacy Card??????????????????????????????????????????"
                },
                "enable-privacy-choose-account": function(e) {
                    return "??????????????????"
                },
                "enable-privacy-add-button": function(e) {
                    return "????????? 1Password"
                },
                "enable-privacy-error": function(e) {
                    return "???????????? Privacy ???????????????????????????"
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "????????????????????????????????????????????????"
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "????????? 1Password ????????????????????????"
                },
                "privacy-error-enable-header": function(e) {
                    return "??????????????????"
                },
                "privacy-error-header": function(e) {
                    return "????????????"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "?????? 1Password ??????????????? Privacy ?????????"
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "?????????????????? Privacy.com ??????????????????????????????????????????????????????????????????"
                },
                "privacy-error-unexpected-error": function(e) {
                    return "?????????????????????????????? support@1password.com"
                },
                "privacy-error-okay-button": function(e) {
                    return "??????"
                },
                "privacy-once": function(e) {
                    return "?????????"
                },
                "privacy-monthly": function(e) {
                    return "??????"
                },
                "privacy-annually": function(e) {
                    return "??????"
                },
                "privacy-transaction": function(e) {
                    return "??????"
                },
                "privacy-forever": function(e) {
                    return "??????"
                },
                "privacy-header": function(e) {
                    return "?????? Privacy ??????"
                },
                "privacy-card-name-label": function(e) {
                    return "????????????"
                },
                "privacy-spend-limit-label": function(e) {
                    return "??????????????????"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "??????"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "??????"
                },
                "privacy-funding-account-label": function(e) {
                    return "????????????"
                },
                "privacy-save-label": function(e) {
                    return "??? 1Password ?????????"
                },
                "privacy-create-button": function(e) {
                    return "???????????????"
                },
                "currency-input-aria-label": function(e) {
                    return "?????????????????????????????????"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "?????????????????????????????????????????? Privacy ?????????"
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password ?????????????????????????????????"
                },
                "privacy-error-vault-locked": function(e) {
                    return "????????????????????????????????????????????????"
                },
                "privacy-error-save-failed": function(e) {
                    return "?????????????????????????????????????????????????????????????????????"
                },
                "privacy-error-empty-card-name": function(e) {
                    return "?????????????????????"
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "???????????????????????????????????????"
                },
                "privacy-error-empty-limit": function(e) {
                    return "??????????????????????????????"
                },
                "privacy-error-limit-too-large": function(e) {
                    return "?????????????????????????????????"
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "???????????? url"
                },
                "privacy-error-http-error": function(e) {
                    return "?????????????????? Privacy???????????????????????????????????????????????????"
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "???????????? Privacy ???????????????????????? API ????????????????????????"
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "??????????????????????????????" + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "??????????????????????????????"
                },
                "privacy-error-brain-error": function(e) {
                    return "????????? Privacy ????????????????????????"
                },
                "privacy-error-default": function(e) {
                    return "?????????????????????"
                },
                "notification-save-in": function(e) {
                    return "?????????"
                },
                "notification-save-secondary": function(e) {
                    return "??? 1Password ????????? " + e.host + " ??????????????????"
                },
                "notification-save-save": function(e) {
                    return "??????"
                }
            }
        },
        "zh-TW": {
            "static-messages": {
                "auth-unfamiliar-website": function(e) {
                    return "??????????????????"
                },
                "auth-type-code-to-fill": function(e) {
                    return "?????? " + e.code + " ????????? " + e.type + " ??????" + e.website + "???"
                },
                "auth-filling-on-website": function(e) {
                    return "??? " + e.website
                },
                "auth-incorrect-code-entered": function(e) {
                    return "???????????????????????????"
                },
                "auth-why-is-this-needed": function(e) {
                    return "???????????????????????????"
                },
                "list-no-items": function(e) {
                    return "????????????????????????"
                },
                "item-save-in-1password": function(e) {
                    return "??? 1Password ?????????"
                },
                "item-use-suggested-password": function(e) {
                    return "?????????????????????"
                },
                "item-create-a-privacy-card": function(e) {
                    return "?????? Privacy ??????"
                },
                "item-type-credit-card": function(e) {
                    return "?????????"
                },
                "item-type-identity": function(e) {
                    return "????????????"
                },
                "item-type-unspecified": function(e) {
                    return "??????"
                },
                categories: function(e) {
                    return "??????"
                },
                "category-suggestions": function(e) {
                    return "??????"
                },
                "category-logins": function(e) {
                    return "??????"
                },
                "category-identities": function(e) {
                    return "????????????"
                },
                "category-credit-cards": function(e) {
                    return "?????????"
                },
                "category-generated-password": function(e) {
                    return "????????????"
                },
                "category-hide-on-this-page": function(e) {
                    return "?????????????????????"
                },
                "locked-unlock-from-toolbar": function(e) {
                    return "?????????????????????????????? 1Password???"
                },
                "locked-press-shortcut-to-unlock": function(e) {
                    return "??? " + e.shortcut + " ????????? 1Password"
                },
                "locked-request-unlock": function(e) {
                    return "?????? 1Password"
                },
                "notification-add-account": function(e) {
                    return "??????????????????"
                },
                "notification-1password": function(e) {
                    return "1Password"
                },
                "notification-add-remove-later": function(e) {
                    return "?????????????????? 1Password ????????????????????????"
                },
                "notification-settings": function(e) {
                    return "??????"
                },
                "notification-add-account-never": function(e) {
                    return "??????"
                },
                "notification-add-account-confirm": function(e) {
                    return "??????"
                },
                "authorize-fill": function(e) {
                    return "?????? OK ?????? " + e.host + " ????????? 1Password ?????????"
                },
                "authorize-fill-privacy": function(e) {
                    return "?????? OK ?????? Privacy ?????????????????? 1Password ??? " + e.host + " ??????"
                },
                Title: function(e) {
                    return "??????"
                },
                "Save-new-Login": function(e) {
                    return "???????????????"
                },
                cancel: function(e) {
                    return "??????"
                },
                close: function(e) {
                    return "??????"
                },
                confirm: function(e) {
                    return "??????"
                },
                Save: function(e) {
                    return "??????"
                },
                Update: function(e) {
                    return "??????"
                },
                "unknown-item": function(e) {
                    return "????????????"
                },
                "save-in": function(e) {
                    return "?????????"
                },
                "select-a-vault": function(e) {
                    return "???????????????"
                },
                "locked-title": function(e) {
                    return "1Password ?????????"
                },
                "locked-message": function(e) {
                    return "??????????????? 1Password ?????????????????????????????????"
                },
                "current-item": function(e) {
                    return "????????????"
                },
                "updated-item": function(e) {
                    return "??????????????????"
                },
                "add-tag": function(e) {
                    return "????????????"
                },
                "remove-tag": function(e) {
                    return "????????????"
                },
                "enable-privacy-header": function(e) {
                    return "????????? 1Password"
                },
                "enable-privacy-description": function(e) {
                    return "?????? 1Password ????????????????????????????????? Privacy Card??????????????????????????????????????????"
                },
                "enable-privacy-choose-account": function(e) {
                    return "????????????"
                },
                "enable-privacy-add-button": function(e) {
                    return "????????? 1Password"
                },
                "enable-privacy-error": function(e) {
                    return "???????????? Privacy ???????????????????????????"
                },
                "enable-privacy-selected-account-locked-account-error": function(e) {
                    return "????????????????????????????????????????????????"
                },
                "enable-privacy-sole-account-locked-account-error": function(e) {
                    return "????????? 1Password ????????????????????????"
                },
                "privacy-error-enable-header": function(e) {
                    return "??????????????????"
                },
                "privacy-error-header": function(e) {
                    return "????????????"
                },
                "privacy-error-no-valid-accounts": function(e) {
                    return "?????? 1Password ??????????????? Privacy ?????????"
                },
                "privacy-error-unable-to-get-funding-accounts": function(e) {
                    return "?????????????????? Privacy.com ??????????????????????????????????????????????????????????????????"
                },
                "privacy-error-unexpected-error": function(e) {
                    return "?????????????????????????????? support@1password.com"
                },
                "privacy-error-okay-button": function(e) {
                    return "OK"
                },
                "privacy-once": function(e) {
                    return "?????????"
                },
                "privacy-monthly": function(e) {
                    return "??????"
                },
                "privacy-annually": function(e) {
                    return "??????"
                },
                "privacy-transaction": function(e) {
                    return "??????"
                },
                "privacy-forever": function(e) {
                    return "??????"
                },
                "privacy-header": function(e) {
                    return "?????? Privacy ??????"
                },
                "privacy-card-name-label": function(e) {
                    return "????????????"
                },
                "privacy-spend-limit-label": function(e) {
                    return "??????????????????"
                },
                "privacy-spend-limit-frequency-label": function(e) {
                    return "??????"
                },
                "privacy-spend-limit-amount-label": function(e) {
                    return "??????"
                },
                "privacy-funding-account-label": function(e) {
                    return "????????????"
                },
                "privacy-save-label": function(e) {
                    return "????????? 1Password"
                },
                "privacy-create-button": function(e) {
                    return "???????????????"
                },
                "currency-input-aria-label": function(e) {
                    return "?????????????????????????????????"
                },
                "privacy-error-integration-disabled": function(e) {
                    return "?????????????????????????????????????????? Privacy ?????????"
                },
                "privacy-error-app-locked": function(e) {
                    return "1Password ?????????????????????????????????"
                },
                "privacy-error-vault-locked": function(e) {
                    return "????????????????????????????????????????????????"
                },
                "privacy-error-save-failed": function(e) {
                    return "?????????????????????????????????????????????????????????????????????"
                },
                "privacy-error-empty-card-name": function(e) {
                    return "?????????????????????"
                },
                "privacy-error-card-name-too-large": function(e) {
                    return "?????????????????????????????????????????????"
                },
                "privacy-error-empty-limit": function(e) {
                    return "??????????????????????????????"
                },
                "privacy-error-limit-too-large": function(e) {
                    return "?????????????????????????????????"
                },
                "privacy-error-page-url-parse-error": function(e) {
                    return "???????????? url"
                },
                "privacy-error-http-error": function(e) {
                    return "?????????????????? Privacy????????????????????????????????????????????????"
                },
                "privacy-error-invalid-api-key": function(e) {
                    return "???????????? Privacy ???????????????????????? API ????????????????????????"
                },
                "privacy-error-api-error-with-message": function(e) {
                    return "??????????????????????????????" + e.message
                },
                "privacy-error-api-error": function(e) {
                    return "??????????????????????????????"
                },
                "privacy-error-brain-error": function(e) {
                    return "????????? Privacy ????????????????????????"
                },
                "privacy-error-default": function(e) {
                    return "?????????????????????"
                },
                "notification-save-in": function(e) {
                    return "?????????"
                },
                "notification-save-secondary": function(e) {
                    return "??? 1Password ????????? " + e.host + " ??????????????????"
                },
                "notification-save-save": function(e) {
                    return "??????"
                }
            }
        }
    };
    function w(e) {
        return "en"in b && "object" == typeof b.en ? void 0 !== b[e] : "en" === e
    }
    function P(e) {
        let t, n = {}, r = {};
        return t = "en"in b && "object" == typeof b.en ? b[e] : b,
        "messages"in t && (r = t.messages),
        "static-messages"in t && (n = t["static-messages"]),
        {
            [e]: Object.assign(Object.assign({}, n), r)
        }
    }
    const k = ()=>function() {
        if ("undefined" != typeof window)
            return window;
        if ("undefined" != typeof globalThis)
            return globalThis;
        throw new Error("unable to locate global object")
    }().crypto.getRandomValues(new Uint32Array(1))[0].toString(36);
    class S {
        constructor() {
            this.request = e=>new Promise((t=>{
                chrome.runtime.sendMessage(e, (e=>{
                    t(e);
                }
                ));
            }
            )),
            this.on = (e,t)=>{
                function n(n, r, i) {
                    return !(!n.name || n.name !== e) && (Promise.resolve(t(n.data)).then(i),
                    !0)
                }
                return chrome.runtime.onMessage.addListener(n),
                n
            }
            ,
            this.off = (e,t)=>{
                chrome.runtime.onMessage.removeListener(t);
            }
            ;
        }
    }
    class E {
        constructor() {
            this.uuid = k(),
            this.callbacks = {},
            this.request = e=>new Promise((t=>{
                const n = E.generateId();
                this.callbacks[n] = t,
                this._sendMessage(Object.assign({
                    callbackId: n
                }, e));
            }
            )),
            this.on = (e,t)=>this._register((n=>{
                n.name && n.name === e && Promise.resolve(t(n.data)).then((e=>{
                    this._sendMessage(Object.assign(Object.assign({}, n), {
                        data: e
                    }));
                }
                ));
            }
            )),
            this.off = (e,t)=>{
                this._deregister(e, t);
            }
            ,
            this.listenForResponses = ()=>{
                this._register((e=>{
                    "callbackId"in e && e.callbackId in this.callbacks && (this.callbacks[e.callbackId](e.data),
                    delete this.callbacks[e.callbackId]);
                }
                ));
            }
            ,
            this._sendMessage.bind(this),
            this._register.bind(this),
            this._deregister.bind(this),
            this.listenForResponses();
        }
    }
    E.generateId = ()=>window.crypto.getRandomValues(new Uint32Array(1))[0];
    class F extends E {
        _sendMessage(e) {
            window.top.postMessage(Object.assign({
                outgoing: !0
            }, e), "*");
        }
        _register(e) {
            function t(t) {
                t.data.outgoing || e(t.data);
            }
            return window.top.addEventListener("message", t),
            t
        }
        _deregister(e, t) {
            window.top.removeEventListener("message", t);
        }
    }
    class I extends E {
        static isSafariAppExtension() {
            return "undefined" != typeof safari && void 0 !== safari.extension
        }
        _sendMessage(e) {
            const t = {
                callbackId: e.callbackId,
                uuid: this.uuid,
                outgoing: !0,
                frameOrigin: window.location.origin,
                message: {
                    name: e.name,
                    data: e.data
                }
            };
            safari.extension.dispatchMessage("message", t);
        }
        _register(e) {
            const t = this.uuid;
            function n(n) {
                const r = n.message;
                !r || r.uuid && r.uuid !== t || e({
                    callbackId: r.callbackId,
                    name: r.message.name,
                    data: r.message.data
                });
            }
            return safari.self.addEventListener("message", n),
            n
        }
        _deregister(e, t) {
            safari.self.removeEventListener("message", t);
        }
    }
    function A(e, t={
        targetParent: !1
    }) {
        return {
            name: "relay-message-to-frames",
            data: {
                message: JSON.stringify(e),
                targetParent: t.targetParent
            }
        }
    }
    const M = new class {
        constructor(e) {
            this.transport = e;
        }
        sayHello() {
            return this.transport.request({
                name: "hello",
                data: void 0
            })
        }
        analyzeFrame(e) {
            return this.transport.request({
                name: "analyze-frame",
                data: e
            })
        }
        getFrameManagerConfiguration() {
            return this.transport.request({
                name: "get-frame-manager-configuration",
                data: void 0
            })
        }
        filterInlineMenu(e) {
            return this.transport.request(A({
                name: "filter-inline-menu",
                data: e
            }))
        }
        focusInlineMenu() {
            return this.transport.request(A({
                name: "focus-inline-menu"
            }))
        }
        requestManagedUnlock() {
            return this.transport.request({
                name: "request-managed-unlock",
                data: void 0
            })
        }
        getSaveObjectPublicKey() {
            return this.transport.request({
                name: "get-save-object-public-key",
                data: void 0
            })
        }
        handleSaveItemRequest(e, t, n) {
            this.transport.request({
                name: "add-save-object",
                data: {
                    type: e,
                    data: t,
                    encrypted: n
                }
            });
        }
        addScrollAndResizeEventListeners() {
            return this.transport.request(A({
                name: "add-scroll-and-resize-event-listeners"
            }, {
                targetParent: !0
            }))
        }
        removeScrollAndResizeEventListeners() {
            return this.transport.request(A({
                name: "remove-scroll-and-resize-event-listeners"
            }))
        }
        resolvePendingFill() {
            return this.transport.request({
                name: "resolve-pending-fill",
                data: void 0
            })
        }
        onHostAppRestarted(e) {
            this.transport.on("host-app-restarted", e);
        }
        onCollectFrameDetails(e) {
            this.transport.on("collect-frame-details", e);
        }
        onPerformFill(e) {
            this.transport.on("perform-fill", (t=>{
                e("response"in t && "status"in t ? t : {
                    response: t,
                    status: "none"
                });
            }
            ));
        }
        editedStateChanged(e) {
            return this.transport.request(A({
                name: "edited-state-changed",
                data: e
            }))
        }
        onLockStateChanged(e) {
            this.transport.on("lock-state-changed", e);
        }
        onFocusPage(e) {
            this.transport.on("focus-page", e);
        }
        onPing(e) {
            this.transport.on("ping", e);
        }
        onAddScrollAndResizeEventListeners(e) {
            this.transport.on("add-scroll-and-resize-event-listeners", e);
        }
        onRemoveScrollAndResizeEventListeners(e) {
            this.transport.on("remove-scroll-and-resize-event-listeners", e);
        }
        onForwardInlineMenuPosition(e) {
            this.transport.on("forward-inline-menu-position", e);
        }
        onGetFrameOrigin(e) {
            this.transport.on("get-frame-origin", e);
        }
        sendFrameOrigin(e) {
            this.transport.request({
                name: "provide-frame-origin",
                data: {
                    uuid: e
                }
            });
        }
        notifyAboutPotentialLoginSubmission(e) {
            this.transport.request({
                name: "potential-login-submitted",
                data: {
                    frame: e
                }
            });
        }
        pageReady() {
            return this.transport.request({
                name: "page-ready",
                data: void 0
            })
        }
        sendInlineMenuOpened() {
            return this.transport.request(A({
                name: "inline-menu-opened"
            }))
        }
        sendInlineMenuClosed() {
            return this.transport.request(A({
                name: "inline-menu-closed"
            }))
        }
        sendVerificationToken(e) {
            return this.transport.request(A({
                name: "verification-token",
                data: e
            }))
        }
        onRequestVerificationToken(e) {
            this.transport.on("request-verification-token", e);
        }
        onUnlockRequested(e) {
            this.transport.on("request-unlock", e);
        }
        onResizeInlineMenu(e) {
            this.transport.on("resize-inline-menu", e);
        }
        onItemDetailValueDragStart(e) {
            this.transport.on("item-detail-value-drag-start", e);
        }
        onItemDetailValueDragEnd(e) {
            this.transport.on("item-detail-value-drag-end", e);
        }
        onRemoveInlineMenu(e) {
            this.transport.on("remove-inline-menu", e);
        }
        onFocusInlineMenuFrame(e) {
            this.transport.on("focus-inline-menu-frame", e);
        }
        onInlineMenuReady(e) {
            this.transport.on("inline-menu-ready", e);
        }
        onKeyDown(e) {
            this.transport.on("key-down", e);
        }
        onPageExcluded(e) {
            this.transport.on("page-excluded", e);
        }
        onRequestFillAuthorization(e) {
            this.transport.on("request-fill-authorization", e);
        }
        onShowSaveDialog(e) {
            this.transport.on("show-save-dialog", e);
        }
        onHideSaveDialog(e) {
            this.transport.on("hide-save-dialog", e);
        }
        forwardInlineMenuPosition(e, t) {
            return this.transport.request(A({
                name: "forward-inline-menu-position",
                data: {
                    configuration: e,
                    matchableFrameWindowProps: t
                }
            }, {
                targetParent: !0
            }))
        }
        removeInlineMenu() {
            return this.transport.request(A({
                name: "remove-inline-menu",
                data: void 0
            }))
        }
        focusInlineMenuFrame() {
            return this.transport.request(A({
                name: "focus-inline-menu-frame"
            }))
        }
        sendKeyDown(e, t) {
            return this.transport.request(A({
                name: "key-down",
                data: {
                    key: e,
                    formEdited: t
                }
            }))
        }
        onInlineMenuOpened(e) {
            this.transport.on("inline-menu-opened", e);
        }
        onInlineMenuClosed(e) {
            this.transport.on("inline-menu-closed", e);
        }
    }
    ("undefined" != typeof chrome && void 0 !== chrome.runtime && void 0 !== chrome.runtime.sendMessage && void 0 !== chrome.runtime.onMessage ? new S : I.isSafariAppExtension() ? new I : new F);
    function O() {
        if ("undefined" != typeof window)
            return window;
        if ("undefined" != typeof globalThis)
            return globalThis;
        throw new Error("unable to locate global object")
    }
    const T = e=>(e.trim().split(/\r?\n/).shift() || "").trim()
      , z = e=>e.replace(/[\r\n\s]+/gm, " ").trim()
      , C = (e={})=>{
        const t = j(document, e);
        return t
    }
      , x = e=>{
        const t = ce(e, q).filter((e=>"hidden" !== e.type));
        return t
    }
      , L = new Set([HTMLScriptElement, HTMLStyleElement])
      , q = ["button", "input", "select"]
      , R = new Set([HTMLBodyElement, HTMLButtonElement, HTMLFormElement, HTMLHeadElement, HTMLIFrameElement, HTMLInputElement, HTMLOptionElement, HTMLScriptElement, HTMLSelectElement, HTMLTableElement, HTMLTableRowElement, HTMLTextAreaElement])
      , D = new Set([HTMLScriptElement, HTMLStyleElement, HTMLButtonElement, HTMLOptionElement, HTMLTextAreaElement])
      , _ = /[^!"\#$%&'()*+,\-./:;<=>?@\[\\\]^_`{|}~0-9\s]/
      , j = (e,t)=>{
        const n = e.defaultView ? e.defaultView : O();
        n.uuid = G();
        const r = [...e.forms]
          , i = x(e);
        i.forEach((e=>{
            e.opid = void 0;
        }
        )),
        le();
        const o = {
            max: t.maxTime || 50,
            start: Date.now()
        }
          , a = void 0 !== t.activeFieldOpid ? t.activeFieldOpid : -1
          , u = i[a] ? i[a] : e.activeElement instanceof HTMLInputElement ? e.activeElement : void 0;
        if (u) {
            const e = i.findIndex((e=>e === u));
            u.opid = -1 !== e ? e : void 0;
        }
        const c = t.activeFormOnly && u && u instanceof HTMLInputElement && u.form || void 0
          , s = N(t, r, c)
          , l = B(t, i, s, o, n, u)
          , d = n.origin && "null" !== n.origin ? n.origin : void 0;
        return {
            direction: e.dir || void 0,
            fields: l,
            forms: [...s.values()],
            origin: d,
            title: e.title || void 0,
            pathName: n.location.pathname,
            uuid: n.uuid
        }
    }
      , N = (e,t,n)=>{
        const r = new Map;
        for (const [i,o] of t.entries()) {
            const t = !!e.activeFormOnly && o !== n;
            r.set(o, U(o, i, t));
        }
        return r
    }
      , U = (e,t,n)=>{
        const r = t;
        if (e.opid = r,
        n)
            return {
                opid: r
            };
        let i, o;
        return ae(e) || (i = Y(e) || void 0,
        o = z(te(e)) || void 0),
        {
            headerText: i,
            htmlAction: X(e) || void 0,
            htmlId: e.getAttribute("id") || void 0,
            htmlMethod: e.getAttribute("method") || void 0,
            htmlName: e.getAttribute("name") || void 0,
            opid: r,
            textContent: o
        }
    }
      , B = (e,t,n,r,i,o)=>{
        const a = o && o.opid ? o.opid : 0;
        let u, c = new Array(t.length), s = 0, l = 0, d = Math.min(t.length, e.maxFields || 200);
        for (s = 0; s < d && ue(r); s++) {
            if (l = ee(s, a, t.length),
            u = t[l],
            e.activeFormOnly && o && u.form !== o.form) {
                d < t.length && d++;
                continue
            }
            const r = u === o
              , f = u.form ? n.get(u.form) : void 0;
            c[l] = K(u, l, r, i, f, e.excludeValues, e.improvedCollectScript);
        }
        for (({fields: c, fieldElements: t} = W(c, t)),
        se(`fields: ${c.length}`),
        l = 0,
        s = 0; s < c.length && ue(r); s++)
            l = ee(s, Math.ceil(c.length / 2), c.length),
            V(t[l], c[l]);
        return c
    }
      , K = (e,t,n,r,i,o,a)=>{
        const u = e instanceof HTMLInputElement
          , c = e instanceof HTMLSelectElement
          , s = t;
        e.opid = s;
        const l = u || c ? e.getAttribute("autocomplete") : void 0
          , d = u && e.checked
          , f = u && e.readOnly
          , p = (u || c) && e.opUserEdited
          , v = (u || c) && e.opAutofilledBy
          , h = u && e.maxLength && e.maxLength > 0 ? e.maxLength : void 0
          , m = u && e.minLength && e.minLength > 0 ? e.minLength : void 0
          , y = u ? e.placeholder : void 0
          , g = c ? Q(e, i) : void 0
          , b = n || oe(e, r, a)
          , w = u ? e.getAttribute("passwordrules") : void 0;
        return {
            autocompleteType: l || void 0,
            formOpid: null == i ? void 0 : i.opid,
            htmlId: e.id || void 0,
            htmlName: e.name || void 0,
            htmlClass: e.className || void 0,
            isActive: n || void 0,
            isAriaDisabled: "true" === e.getAttribute("aria-disabled") || void 0,
            isAriaHasPopup: "true" === e.getAttribute("aria-haspopup") || void 0,
            isAriaHidden: "true" === e.getAttribute("aria-hidden") || void 0,
            isChecked: d || void 0,
            isDisabled: e.disabled || void 0,
            isHidden: !b || void 0,
            isReadOnly: f || void 0,
            isUserEdited: p || void 0,
            autofilledBy: v || void 0,
            maxLength: h,
            minLength: m,
            opid: s,
            placeholder: y || void 0,
            selectOptions: g,
            tabIndex: e.tabIndex || void 0,
            title: e.title || void 0,
            type: H(e.type),
            value: o ? void 0 : ne(e) || void 0,
            passwordRules: w || void 0
        }
    }
      , H = e=>{
        switch (e) {
        case "button":
            return "button";
        case "checkbox":
            return "checkbox";
        case "color":
            return "color";
        case "date":
            return "date";
        case "datetime-local":
            return "datetime-local";
        case "email":
            return "email";
        case "file":
            return "file";
        case "hidden":
            return "hidden";
        case "image":
            return "image";
        case "month":
            return "month";
        case "number":
            return "number";
        case "password":
            return "password";
        case "radio":
            return "radio";
        case "range":
            return "range";
        case "reset":
            return "reset";
        case "search":
            return "search";
        case "submit":
            return "submit";
        case "tel":
            return "tel";
        case "text":
            return "text";
        case "time":
            return "time";
        case "url":
            return "url";
        case "week":
            return "week";
        case "select-one":
            return "select-one";
        case "select-multiple":
            return "select-multiple";
        case "textarea":
            return "textarea";
        default:
            return "unknown"
        }
    }
      , V = (e,t)=>{
        t.label = J(e) || void 0,
        t.labelAria = re(e) || void 0,
        t.labelData = e.getAttribute("data-label") || void 0,
        t.labelBefore = Y(e) || void 0,
        t.labelAfter = Z(e) || void 0;
    }
      , W = (e,t)=>(e = e.filter(Boolean),
    t = t.filter((e=>void 0 !== e.opid)),
    {
        fields: e,
        fieldElements: t
    });
    class $ {
        constructor(e, t) {
            console.assert(oe(e), "element must be visible for correct tree traversal"),
            this.direction = t,
            this.pauseNode = this.nextPauseNode(e),
            this.walker = $.createTreeWalker(e, t);
        }
        static createTreeWalker(e, t) {
            const n = document.body
              , r = NodeFilter.SHOW_ELEMENT | NodeFilter.SHOW_TEXT
              , i = {
                acceptNode: e=>e.nodeType === Node.TEXT_NODE && e.parentElement && D.has(e.parentElement.constructor) ? NodeFilter.FILTER_SKIP : NodeFilter.FILTER_ACCEPT
            }
              , o = document.createTreeWalker(n, r, i);
            return o.currentNode = e,
            1 === t && e.nextSibling && (o.currentNode = e.nextSibling,
            o.previousNode()),
            o
        }
        normalizeStrings(e) {
            return e.length ? (0 === this.direction && (e = e.reverse()),
            e.join(" ").trim().replace(/\s+/g, " ")) : ""
        }
        advanceWalker() {
            return 0 === this.direction ? !!this.walker.previousNode() : !!this.walker.nextNode()
        }
        nextPauseNode(e) {
            if (0 === this.direction)
                return e.parentElement;
            for (; e.parentElement; ) {
                if (e.parentElement.nextSibling)
                    return e.parentElement.nextSibling;
                e = e.parentElement;
            }
            return null
        }
        shouldPauseAtNode(e) {
            return e === this.pauseNode && (this.pauseNode = this.nextPauseNode(this.pauseNode),
            !0)
        }
        shouldStopAtNode(e) {
            if (1 === this.direction && e === this.pauseNode)
                return !0;
            if (e.nodeType !== Node.ELEMENT_NODE)
                return !1;
            const t = e;
            return t instanceof HTMLInputElement && ("text" === t.type || "password" === t.type) ? oe(t) : ie(t, R)
        }
        findLabelText() {
            const e = [];
            for (; this.advanceWalker(); ) {
                const t = this.walker.currentNode;
                if (this.shouldStopAtNode(t))
                    break;
                if (this.shouldPauseAtNode(t)) {
                    if (e.length) {
                        const t = this.normalizeStrings(e);
                        return t.length > 150 ? "" : t
                    }
                    continue
                }
                const n = t instanceof HTMLElement ? t : t.parentElement;
                if (!oe(n)) {
                    this.skipInvisibleElement();
                    continue
                }
                if (t.nodeType !== Node.TEXT_NODE)
                    continue;
                const r = t.nodeValue;
                r && _.test(r) && e.push(r);
            }
            const t = this.normalizeStrings(e);
            return t.length > 150 ? "" : t
        }
        skipInvisibleElement() {
            let e = this.walker.currentNode;
            if (0 === this.direction)
                for (; e.parentElement && !oe(e.parentElement); )
                    e = e.parentElement;
            else if (e.firstChild)
                for (e = e.firstChild; e.nextSibling || e.firstChild; )
                    e = e.nextSibling || e.firstChild;
            this.walker.currentNode = e;
        }
    }
    const G = ()=>O().crypto.getRandomValues(new Uint32Array(1))[0].toString(36)
      , X = e=>{
        const t = e.ownerDocument ? e.ownerDocument.defaultView : void 0;
        if (t) {
            const n = e.getAttribute("action");
            return n ? new URL(n,t.location.href).href : t.location.href
        }
        return ""
    }
      , J = e=>e.labels && e.labels.length > 0 ? [...e.labels].map((e=>T(te(e)))).join(" ") : ""
      , Y = e=>{
        if (!oe(e))
            return "";
        return new $(e,0).findLabelText()
    }
      , Z = e=>{
        if (!oe(e))
            return "";
        return new $(e,1).findLabelText()
    }
      , Q = (e,t)=>{
        const {options: n} = e
          , r = [];
        return n && n.length > 0 && [...n].forEach((e=>{
            r.push(z(e.text)),
            r.push(e.value);
        }
        )),
        t && t.textContent && (t.textContent = z(t.textContent.replace(z(te(e)), ""))),
        r.length ? r : void 0
    }
      , ee = (e,t,n)=>{
        const r = Math.ceil(e / 2)
          , i = t - r
          , o = t + r;
        return i >= 0 && o < n ? e % 2 ? i : o : i < 0 ? e : n - e - 1
    }
      , te = e=>ie(e, L) ? "" : e.innerText || e.textContent || ""
      , ne = e=>{
        const {type: t, value: n} = e;
        switch (t) {
        case "submit":
        case "button":
        case "reset":
            return "" === n ? T(te(e)) : n;
        default:
            return n
        }
    }
      , re = e=>{
        const t = e.getAttribute("aria-label");
        if (t)
            return t;
        const n = e.getAttribute("aria-labelledby");
        if (!n)
            return "";
        let r = "";
        for (const e of n.split(" ")) {
            const t = document.getElementById(e);
            t && (r += te(t) + " ");
        }
        return z(r)
    }
      , ie = (e,t)=>!!e && t.has(e.constructor)
      , oe = (e,t,n)=>{
        if (!e)
            return !1;
        const r = n && t ? t.getComputedStyle(e) : e.style;
        return "hidden" !== r.visibility && "none" !== r.display && !!(e.offsetWidth || e.offsetHeight || e.getClientRects().length)
    }
      , ae = e=>e.parentElement === document.body
      , ue = e=>Date.now() - e.start < e.max
      , ce = (e,t)=>[...e.querySelectorAll(t.join())]
      , se = e=>{}
      , le = e=>{}
      , de = "data-com-onepassword-filled"
      , fe = ["button", "input", "select"]
      , pe = async(e,t)=>{
        if (!ke(e, t.uuid) || !we(t.allowedTargets) || !Pe(t.allowUnsafeHttp))
            return;
        const n = Se();
        let r = n.slice();
        for (const e of t.operations) {
            const t = ve(e, r);
            r = r.slice(t + 1),
            await ze();
        }
        t.postFill && (ve(t.postFill, n),
        await ze());
    }
      , ve = (e,t)=>{
        const {action: n, opid: r, querySelector: i, value: o} = e;
        let a = 0;
        switch (n) {
        case "clickOpid":
            if (void 0 === r)
                break;
            a = he(t, r);
            break;
        case "fillOpid":
            if (void 0 === r)
                break;
            a = me(t, r, o);
            break;
        case "fillQuerySelector":
            if (!i)
                break;
            ye(i, o);
            break;
        case "focusOpid":
            if (void 0 === r)
                break;
            a = ge(t, r);
        }
        return a
    }
      , he = (e,t)=>{
        const n = e.findIndex((e=>e.opid === t))
          , r = e[n];
        return r && r.click(),
        n
    }
      , me = (e,t,n)=>{
        const r = e.findIndex((e=>e.opid === t))
          , i = e[r];
        return i && Oe(i, n),
        r
    }
      , ye = (e,t)=>{
        const n = document.querySelector(e);
        n && Oe(n, t);
    }
      , ge = (e,t)=>{
        const n = e.findIndex((e=>e.opid === t))
          , r = e[n];
        return r && be(r, !0),
        n
    }
      , be = (e,t)=>{
        const n = e.value;
        "checkbox" !== e.type && e.click(),
        e.focus(),
        t && e.value !== n && (e.value = n);
    }
      , we = e=>{
        if ("Anywhere" === e.type)
            return !0;
        if (!window.origin || "null" === window.origin)
            return !1;
        const {hostname: t} = window.location;
        return e.data.filter((e=>"Url" === e.type)).some((e=>e.data === t || t.endsWith(`.${e.data}`)))
    }
      , Pe = (e=!1)=>"https:" === window.location.protocol || (!!e || confirm("1Password warning: This is an unsecured (HTTP) page, and any information you submit can potentially be seen and changed by others. This Login was originally saved on a secure (HTTPS) page.\r\n\r\nDo you still wish to fill this login?"))
      , ke = (e,t)=>!!e && e === t
      , Se = ()=>Fe(document, fe).filter((e=>"hidden" !== e.type))
      , Ee = e=>{
        e.dispatchEvent(new KeyboardEvent("keydown")),
        e.dispatchEvent(new KeyboardEvent("keypress")),
        e.dispatchEvent(new KeyboardEvent("keyup"));
    }
      , Fe = (e,t)=>[...e.querySelectorAll(t.join())]
      , Ie = e=>{
        var t, n;
        const r = null !== (t = window.getComputedStyle(e).backgroundColor) && void 0 !== t ? t : ""
          , i = Ae(r);
        if (i.alpha > .5 && "transparent" !== r) {
            const t = Me(i) > .5 ? "light" : "dark";
            return e.setAttribute(de, t)
        }
        const o = null !== (n = window.getComputedStyle(e).color) && void 0 !== n ? n : ""
          , a = Ae(o)
          , u = Me(a) < .5 ? "light" : "dark";
        e.setAttribute(de, u);
    }
      , Ae = e=>{
        let t = {
            red: 0,
            green: 0,
            blue: 0,
            alpha: 0
        };
        if (!e.startsWith("rgb"))
            return t;
        const n = e.split("(")[1].split(")")[0].split(",").map((e=>e.trim()));
        return n && n.length >= 3 && (t = {
            red: Number(n[0]),
            green: Number(n[1]),
            blue: Number(n[2]),
            alpha: 4 === n.length ? Number(n[3]) : 1
        }),
        t
    }
      , Me = e=>(.2126 * e.red + .7152 * e.green + .0722 * e.blue) / 255
      , Oe = (e,t)=>{
        "checkbox" === e.type ? e.checked !== !!t && e.click() : (Te(e),
        e.value = t,
        (e=>{
            const t = e.value;
            Ee(e),
            e.dispatchEvent(new Event("input",{
                bubbles: !0,
                cancelable: !0
            })),
            e.dispatchEvent(new Event("change",{
                bubbles: !0,
                cancelable: !0
            })),
            e.setAttribute(de, "");
            const n = ()=>{
                e.hasAttribute(de) && e.removeAttribute(de),
                e.removeEventListener("input", n);
            }
            ;
            e.addEventListener("input", n),
            Ie(e),
            "" === e.value && (e.value = t),
            e.blur();
        }
        )(e)),
        e.opUserEdited = !0,
        e.opAutofilledBy = "us";
    }
      , Te = e=>{
        const t = e.value;
        be(e),
        Ee(e),
        "" === e.value && (e.value = t);
    }
      , ze = ()=>new Promise((e=>{
        setTimeout((()=>e()), 1);
    }
    ));
    var Ce = function(e) {
        var t, n = e.Symbol;
        return "function" == typeof n ? n.observable ? t = n.observable : (t = n("observable"),
        n.observable = t) : t = "@@observable",
        t
    }("undefined" != typeof self ? self : "undefined" != typeof window ? window : "undefined" != typeof global ? global : "undefined" != typeof module ? module : Function("return this")())
      , xe = function() {
        return Math.random().toString(36).substring(7).split("").join(".")
    }
      , Le = {
        INIT: "@@redux/INIT" + xe(),
        REPLACE: "@@redux/REPLACE" + xe(),
        PROBE_UNKNOWN_ACTION: function() {
            return "@@redux/PROBE_UNKNOWN_ACTION" + xe()
        }
    };
    function qe(e) {
        if ("object" != typeof e || null === e)
            return !1;
        for (var t = e; null !== Object.getPrototypeOf(t); )
            t = Object.getPrototypeOf(t);
        return Object.getPrototypeOf(e) === t
    }
    const Re = {
        locked: !0,
        basePath: "/",
        configured: !1,
        activeField: void 0,
        activeFieldAnalysis: void 0,
        activeFieldRect: void 0,
        activeFieldDir: "ltr",
        activeFieldPaddingX: 0,
        autoshowMenu: !1,
        inlineMenuOpen: !1,
        inlineMenuToken: void 0,
        frameIdentifier: window.crypto.getRandomValues(new Uint32Array(1))[0].toString(36),
        fillInProgress: !1,
        inlineDisabled: !1,
        locale: "en",
        fillPendingUserInteraction: !1,
        canRequestUnlock: !1,
        showOptionsMenu: !0,
        appTheme: "system"
    };
    const De = function e(t, n, r) {
        var i;
        if ("function" == typeof n && "function" == typeof r || "function" == typeof r && "function" == typeof arguments[3])
            throw new Error("It looks like you are passing several store enhancers to createStore(). This is not supported. Instead, compose them together to a single function.");
        if ("function" == typeof n && void 0 === r && (r = n,
        n = void 0),
        void 0 !== r) {
            if ("function" != typeof r)
                throw new Error("Expected the enhancer to be a function.");
            return r(e)(t, n)
        }
        if ("function" != typeof t)
            throw new Error("Expected the reducer to be a function.");
        var o = t
          , a = n
          , u = []
          , c = u
          , s = !1;
        function l() {
            c === u && (c = u.slice());
        }
        function d() {
            if (s)
                throw new Error("You may not call store.getState() while the reducer is executing. The reducer has already received the state as an argument. Pass it down from the top reducer instead of reading it from the store.");
            return a
        }
        function f(e) {
            if ("function" != typeof e)
                throw new Error("Expected the listener to be a function.");
            if (s)
                throw new Error("You may not call store.subscribe() while the reducer is executing. If you would like to be notified after the store has been updated, subscribe from a component and invoke store.getState() in the callback to access the latest state. See https://redux.js.org/api-reference/store#subscribelistener for more details.");
            var t = !0;
            return l(),
            c.push(e),
            function() {
                if (t) {
                    if (s)
                        throw new Error("You may not unsubscribe from a store listener while the reducer is executing. See https://redux.js.org/api-reference/store#subscribelistener for more details.");
                    t = !1,
                    l();
                    var n = c.indexOf(e);
                    c.splice(n, 1),
                    u = null;
                }
            }
        }
        function p(e) {
            if (!qe(e))
                throw new Error("Actions must be plain objects. Use custom middleware for async actions.");
            if (void 0 === e.type)
                throw new Error('Actions may not have an undefined "type" property. Have you misspelled a constant?');
            if (s)
                throw new Error("Reducers may not dispatch actions.");
            try {
                s = !0,
                a = o(a, e);
            } finally {
                s = !1;
            }
            for (var t = u = c, n = 0; n < t.length; n++) {
                (0,
                t[n])();
            }
            return e
        }
        function v(e) {
            if ("function" != typeof e)
                throw new Error("Expected the nextReducer to be a function.");
            o = e,
            p({
                type: Le.REPLACE
            });
        }
        function h() {
            var e, t = f;
            return (e = {
                subscribe: function(e) {
                    if ("object" != typeof e || null === e)
                        throw new TypeError("Expected the observer to be an object.");
                    function n() {
                        e.next && e.next(d());
                    }
                    return n(),
                    {
                        unsubscribe: t(n)
                    }
                }
            })[Ce] = function() {
                return this
            }
            ,
            e
        }
        return p({
            type: Le.INIT
        }),
        (i = {
            dispatch: p,
            subscribe: f,
            getState: d,
            replaceReducer: v
        })[Ce] = h,
        i
    }((function(e, t) {
        if (!e)
            return Re;
        switch (t.type) {
        case "configure":
            return Object.assign(Object.assign(Object.assign({}, e), t.payload), {
                configured: !0
            });
        case "lock-state-changed":
            return Object.assign(Object.assign({}, e), {
                locked: t.payload
            });
        case "set-active-field":
            const n = {
                direction: "ltr",
                paddingLeft: "0",
                paddingRight: "0"
            }
              , r = t.payload ? window.getComputedStyle(t.payload) : n
              , i = "rtl" === r.direction ? "rtl" : "ltr"
              , o = parseFloat("rtl" === i ? r.paddingLeft : r.paddingRight);
            return Object.assign(Object.assign({}, e), {
                activeField: t.payload,
                activeFieldAnalysis: t.payload ? e.activeFieldAnalysis : void 0,
                activeFieldRect: t.payload ? t.payload.getBoundingClientRect() : void 0,
                activeFieldDir: i,
                activeFieldPaddingX: o
            });
        case "set-active-field-analysis":
            return Object.assign(Object.assign({}, e), {
                activeFieldAnalysis: t.payload
            });
        case "set-autoshow-menu":
            return Object.assign(Object.assign({}, e), {
                autoshowMenu: t.payload
            });
        case "set-inline-menu-open":
            return Object.assign(Object.assign({}, e), {
                inlineMenuOpen: t.payload
            });
        case "set-inline-menu-token":
            return Object.assign(Object.assign({}, e), {
                inlineMenuToken: t.payload
            });
        case "fill-start":
            return Object.assign(Object.assign({}, e), {
                fillInProgress: !0,
                fillPendingUserInteraction: t.payload
            });
        case "fill-end":
            return Object.assign(Object.assign({}, e), {
                fillInProgress: !1
            });
        case "set-page-excluded":
            return Object.assign(Object.assign({}, e), {
                inlineDisabled: t.payload
            });
        case "cancel-pending-fill":
            return Object.assign(Object.assign({}, e), {
                fillPendingUserInteraction: !1
            });
        default:
            return e
        }
    }
    ))
      , _e = (e,t)=>((e,t)=>function(e, t, n) {
        let r;
        function i() {
            const i = t(e.getState());
            i !== r && (r = i,
            n(r));
        }
        const o = e.subscribe(i);
        return i(),
        o
    }(De, e, t))((t=>t[e]), t);
    class je {
        constructor() {
            this.draw = ()=>{
                this.createShadowRoot(),
                this.createStatus(),
                this.createButton(),
                this.setButtonStyles(),
                this.addShadowHostToDOM();
            }
            ,
            this.erase = ()=>{
                var e;
                (null === (e = this.shadowRoot) || void 0 === e ? void 0 : e.host.parentElement) && document.body.removeChild(this.shadowRoot.host);
            }
            ,
            this.createShadowRoot = ()=>{
                !this.shadowRoot && document.body && (this.shadowRoot = document.createElement("com-1password-button").attachShadow({
                    mode: "closed"
                }),
                this.shadowRoot.addEventListener("mousedown", (e=>{
                    e.stopImmediatePropagation(),
                    e.preventDefault();
                }
                )),
                this.shadowRoot.addEventListener("click", (e=>{
                    var t;
                    e.target && (null === (t = this.onClick) || void 0 === t || t.call(this));
                }
                )));
            }
            ,
            this.createButton = ()=>{
                this.shadowRoot && !this.button && (this.button = document.createElement("button"),
                this.button.id = "op-button",
                this.shadowRoot.appendChild(this.button));
            }
            ,
            this.createStatus = ()=>{
                this.shadowRoot && !this.status && (this.status = document.createElement("div"),
                this.status.id = "op-status",
                this.status.setAttribute("role", "status"),
                this.status.innerText = "1Password menu available. Press the down arrow key to select.",
                this.status.style.cssText = "\n        all: initial;\n        position: absolute;\n        top: -1000px;\n        left: -1000px;\n        opacity: 0;\n        width: 0;\n        height: 0;\n      ",
                this.shadowRoot.appendChild(this.status));
            }
            ,
            this.setButtonStyles = ()=>{
                const {activeFieldRect: e} = De.getState();
                if (!this.button || !e)
                    return;
                const t = e.top
                  , n = e.left
                  , [r] = this.calculateSize(e.height)
                  , i = (e.height - r) / 2
                  , {basePath: o, activeFieldDir: a, activeFieldPaddingX: u} = De.getState()
                  , c = Math.max(i, u)
                  , s = t + i
                  , l = e.width - r - c
                  , d = n + ("rtl" === a ? c : l)
                  , f = De.getState().locked ? "locked" : "unlocked";
                this.button.style.cssText = `\n        all: initial;\n        position: fixed;\n        z-index: 2147483647;\n        top: ${s}px;\n\t\t\t\tleft: ${d}px;\n        min-width: 12px;\n        width: ${r}px;\n        max-width: 24px;\n        min-height: 12px;\n        height: ${r}px;\n        max-height: 24px;\n        background-image: url(${o}/images/icons/app_icon-light_bg-color-${f}-${r}.svg);\n        background-size: cover;\n        background-repeat: no-repeat;\n        border: none;\n        outline: 0;\n        cursor: pointer;\n      `;
            }
            ,
            this.calculateSize = e=>e >= 38 ? [je.large, "large"] : e < je.medium + 4 ? [je.small, "small"] : [je.medium, "medium"],
            _e("activeField", (e=>{
                e ? this.draw() : this.erase();
            }
            )),
            _e("locked", (()=>{
                this.setButtonStyles();
            }
            )),
            _e("inlineDisabled", (e=>{
                e && this.erase();
            }
            ));
        }
        addShadowHostToDOM() {
            if (!this.shadowRoot || this.shadowRoot.host.parentElement)
                return;
            const e = document.getElementsByTagName("com-1password-save-dialog")[0];
            e ? document.body.insertBefore(this.shadowRoot.host, e) : document.body.appendChild(this.shadowRoot.host);
        }
    }
    je.small = 12,
    je.medium = 16,
    je.large = 24;
    var Ne = function(e) {
        var t = typeof e;
        return null != e && ("object" == t || "function" == t)
    }
      , Ue = "object" == typeof a && a && a.Object === Object && a
      , Be = "object" == typeof self && self && self.Object === Object && self
      , Ke = Ue || Be || Function("return this")()
      , He = function() {
        return Ke.Date.now()
    }
      , Ve = /\s/;
    var We = function(e) {
        for (var t = e.length; t-- && Ve.test(e.charAt(t)); )
            ;
        return t
    }
      , $e = /^\s+/;
    var Ge = function(e) {
        return e ? e.slice(0, We(e) + 1).replace($e, "") : e
    }
      , Xe = Ke.Symbol
      , Je = Object.prototype
      , Ye = Je.hasOwnProperty
      , Ze = Je.toString
      , Qe = Xe ? Xe.toStringTag : void 0;
    var et = function(e) {
        var t = Ye.call(e, Qe)
          , n = e[Qe];
        try {
            e[Qe] = void 0;
            var r = !0;
        } catch (e) {}
        var i = Ze.call(e);
        return r && (t ? e[Qe] = n : delete e[Qe]),
        i
    }
      , tt = Object.prototype.toString;
    var nt = function(e) {
        return tt.call(e)
    }
      , rt = Xe ? Xe.toStringTag : void 0;
    var it = function(e) {
        return null == e ? void 0 === e ? "[object Undefined]" : "[object Null]" : rt && rt in Object(e) ? et(e) : nt(e)
    };
    var ot = function(e) {
        return null != e && "object" == typeof e
    };
    var at = function(e) {
        return "symbol" == typeof e || ot(e) && "[object Symbol]" == it(e)
    }
      , ut = /^[-+]0x[0-9a-f]+$/i
      , ct = /^0b[01]+$/i
      , st = /^0o[0-7]+$/i
      , lt = parseInt;
    var dt = function(e) {
        if ("number" == typeof e)
            return e;
        if (at(e))
            return NaN;
        if (Ne(e)) {
            var t = "function" == typeof e.valueOf ? e.valueOf() : e;
            e = Ne(t) ? t + "" : t;
        }
        if ("string" != typeof e)
            return 0 === e ? e : +e;
        e = Ge(e);
        var n = ct.test(e);
        return n || st.test(e) ? lt(e.slice(2), n ? 2 : 8) : ut.test(e) ? NaN : +e
    }
      , ft = Math.max
      , pt = Math.min;
    var vt = function(e, t, n) {
        var r, i, o, a, u, c, s = 0, l = !1, d = !1, f = !0;
        if ("function" != typeof e)
            throw new TypeError("Expected a function");
        function p(t) {
            var n = r
              , o = i;
            return r = i = void 0,
            s = t,
            a = e.apply(o, n)
        }
        function v(e) {
            return s = e,
            u = setTimeout(m, t),
            l ? p(e) : a
        }
        function h(e) {
            var n = e - c;
            return void 0 === c || n >= t || n < 0 || d && e - s >= o
        }
        function m() {
            var e = He();
            if (h(e))
                return y(e);
            u = setTimeout(m, function(e) {
                var n = t - (e - c);
                return d ? pt(n, o - (e - s)) : n
            }(e));
        }
        function y(e) {
            return u = void 0,
            f && r ? p(e) : (r = i = void 0,
            a)
        }
        function g() {
            var e = He()
              , n = h(e);
            if (r = arguments,
            i = this,
            c = e,
            n) {
                if (void 0 === u)
                    return v(c);
                if (d)
                    return clearTimeout(u),
                    u = setTimeout(m, t),
                    p(c)
            }
            return void 0 === u && (u = setTimeout(m, t)),
            a
        }
        return t = dt(t) || 0,
        Ne(n) && (l = !!n.leading,
        o = (d = "maxWait"in n) ? ft(dt(n.maxWait) || 0, t) : o,
        f = "trailing"in n ? !!n.trailing : f),
        g.cancel = function() {
            void 0 !== u && clearTimeout(u),
            s = 0,
            r = c = i = u = void 0;
        }
        ,
        g.flush = function() {
            return void 0 === u ? a : y(He())
        }
        ,
        g
    };
    var ht, mt = function(e, t, n) {
        var r = !0
          , i = !0;
        if ("function" != typeof e)
            throw new TypeError("Expected a function");
        return Ne(n) && (r = "leading"in n ? !!n.leading : r,
        i = "trailing"in n ? !!n.trailing : i),
        vt(e, t, {
            leading: r,
            maxWait: t,
            trailing: i
        })
    };
    class yt {
        constructor() {
            this.ensureFillableField = ({configured: e=De.getState().configured, fillInProgress: t=De.getState().fillInProgress, inlineDisabled: n=De.getState().inlineDisabled, target: r=document.activeElement},i)=>{
                e && !n && !t && this.isFillableField(r) && (null == i || i(r));
            }
            ,
            this.isFillableField = e=>e instanceof HTMLInputElement && ["text", "email", "number", "password", "url", "tel", "search"].includes(e.type.toLowerCase()) && !e.readOnly && !e.disabled,
            this.onFocus = e=>{
                var t;
                const {target: n} = e;
                this.isOwnAsset(n) || (De.getState().activeField && (null === (t = this.onInputBlur) || void 0 === t || t.call(this)),
                this.ensureFillableField({
                    inlineDisabled: !1,
                    target: n
                }, this.resolvePendingFill));
            }
            ,
            this.onAnimationStart = e=>{
                const {target: t} = e;
                "onautofillstart" === e.animationName && (t instanceof HTMLInputElement || t instanceof HTMLSelectElement) && (t.opUserEdited = !0,
                t.opAutofilledBy = "browser");
            }
            ,
            this.onInput = e=>{
                var t;
                const {target: n} = e
                  , {activeField: r} = De.getState();
                e.isTrusted && (n instanceof HTMLInputElement || n instanceof HTMLSelectElement) && (n.opUserEdited = !0,
                n.opAutofilledBy && (n.opAutofilledBy = void 0)),
                r && n === r && (null === (t = this.onActiveInputChange) || void 0 === t || t.call(this, n));
            }
            ,
            this.onKeyDown = e=>{
                var t;
                const {activeField: n} = De.getState();
                n && e.target === n && (e.metaKey || e.ctrlKey || e.shiftKey || e.altKey || ["ArrowUp", "ArrowDown", "Escape"].includes(e.key) && (e.preventDefault(),
                null === (t = this.onActiveInputKeyDown) || void 0 === t || t.call(this, e.key)));
            }
            ,
            this.onMouseDown = e=>{
                var t;
                const {target: n} = e;
                this.isOwnAsset(n) || (De.getState().activeField || n !== document.activeElement ? null === (t = this.onInputBlur) || void 0 === t || t.call(this) : this.ensureFillableField({
                    target: n
                }, this.onInputFocus));
            }
            ,
            this.resolvePendingFill = e=>{
                const {fillPendingUserInteraction: t, inlineDisabled: n} = De.getState()
                  , r = e=>{
                    var t;
                    n || null === (t = this.onInputFocus) || void 0 === t || t.call(this, e);
                }
                ;
                t ? M.resolvePendingFill().then((t=>{
                    t || (De.dispatch({
                        type: "cancel-pending-fill"
                    }),
                    r(e));
                }
                )) : r(e);
            }
            ,
            this.activeFieldVisibilityObserver = new IntersectionObserver((e=>{
                var t;
                for (const n of e) {
                    const {width: e, height: r} = n.boundingClientRect;
                    n.isIntersecting || 0 !== e || 0 !== r || null === (t = this.onInputBlur) || void 0 === t || t.call(this);
                }
            }
            )),
            window.addEventListener("focusin", mt(this.onFocus, 10), !0),
            window.addEventListener("keydown", mt(this.onKeyDown, 10), !0),
            window.addEventListener("animationstart", this.onAnimationStart, !0),
            window.addEventListener("input", this.onInput, !0),
            window.addEventListener("mousedown", this.onMouseDown, !0),
            _e("inlineDisabled", (e=>{
                this.ensureFillableField({
                    inlineDisabled: e
                }, this.onInputFocus);
            }
            )),
            _e("configured", (e=>{
                this.ensureFillableField({
                    configured: e,
                    inlineDisabled: !1
                }, this.resolvePendingFill);
            }
            )),
            _e("fillInProgress", (e=>{
                this.ensureFillableField({
                    fillInProgress: e
                }, this.onInputFocus);
            }
            )),
            _e("activeField", (e=>{
                e ? this.activeFieldVisibilityObserver.observe(e) : this.activeFieldVisibilityObserver.disconnect();
            }
            ));
        }
        isOwnAsset(e) {
            const {activeField: t} = De.getState();
            return !!(t && e === t || e instanceof Element && ["COM-1PASSWORD-BUTTON", "COM-1PASSWORD-MENU"].includes(e.tagName))
        }
    }
    class gt {
        constructor() {
            this._isFieldOrFormEdited = e=>this._getFormState(e).editedFieldCount > 0,
            this._onInputFocus = e=>{
                this._shouldDecorate(e) && this._getSuggestions(e).then((t=>{
                    t.suggestions.length > 0 && (De.dispatch({
                        type: "set-active-field",
                        payload: e
                    }),
                    De.dispatch({
                        type: "set-active-field-analysis",
                        payload: t
                    }),
                    this._ensureFieldIsTracked(e),
                    this.onActiveInputFocus && this.onActiveInputFocus(this.isEdited));
                }
                )).catch((()=>{}
                ));
            }
            ,
            this._onInputBlur = ()=>{
                this.onActiveInputBlur && this.onActiveInputBlur();
            }
            ,
            this._ensureFormIsTracked = e=>{
                let t = this.trackedForms.get(e);
                return t || (t = {
                    initialValues: new WeakMap,
                    editedFields: new WeakSet,
                    editedFieldCount: 0
                },
                this.trackedForms.set(e, t)),
                t
            }
            ,
            this._ensureStandaloneFieldIsTracked = e=>{
                let t = this.trackedStandaloneFields.get(e);
                return t || (t = {
                    initialValues: new WeakMap,
                    editedFields: new WeakSet,
                    editedFieldCount: 0
                },
                this.trackedStandaloneFields.set(e, t)),
                t
            }
            ,
            this._getFormState = e=>e.form ? this._ensureFormIsTracked(e.form) : this._ensureStandaloneFieldIsTracked(e),
            this._ensureFieldIsTracked = e=>{
                const t = this._getFormState(e);
                t.initialValues.has(e) || t.initialValues.set(e, e.value);
            }
            ,
            this._onActiveInputKeyDown = e=>{
                this.onActiveInputKeyDown && this.onActiveInputKeyDown(e, this.isEdited);
            }
            ,
            this._onActiveInputChange = e=>{
                const t = this._getFormState(e);
                if (De.getState().fillInProgress)
                    return void (t.editedFields.has(e) && (t.editedFields.delete(e),
                    t.editedFieldCount--));
                const n = this._isFieldOrFormEdited(e);
                t.initialValues.get(e) === e.value ? t.editedFields.has(e) && (t.editedFields.delete(e),
                t.editedFieldCount--) : t.editedFields.has(e) || (t.editedFields.add(e),
                t.editedFieldCount++);
                const r = this._isFieldOrFormEdited(e);
                n !== r && M.editedStateChanged(r),
                this.onActiveInputChange && this.onActiveInputChange(e);
            }
            ,
            this._onButtonClick = ()=>{
                this.onButtonClick && this.onButtonClick(this.isEdited);
            }
            ,
            this._shouldDecorate = e=>e.offsetWidth >= 50 && e.offsetHeight >= 5 && e.offsetWidth >= 2 * e.offsetHeight,
            this._getSuggestions = async e=>{
                const t = this.trackedFieldAnalysis.get(e);
                if (t)
                    return t;
                if (this.trackedFieldAnalysis.has(e))
                    throw new Error("suggestions are already being computed for field");
                this.trackedFieldAnalysis.set(e, void 0);
                const n = await M.analyzeFrame(C({
                    activeFormOnly: !0
                }));
                return this.trackedFieldAnalysis.set(e, n),
                n
            }
            ,
            this.inputManager = new yt,
            this.inputManager.onInputFocus = this._onInputFocus,
            this.inputManager.onInputBlur = this._onInputBlur,
            this.inputManager.onActiveInputKeyDown = this._onActiveInputKeyDown,
            this.inputManager.onActiveInputChange = this._onActiveInputChange,
            this.buttonManager = new je,
            this.buttonManager.onClick = this._onButtonClick,
            this.trackedForms = new WeakMap,
            this.trackedStandaloneFields = new WeakMap,
            this.trackedFieldAnalysis = new WeakMap,
            M.onHideSaveDialog(this._onInputBlur);
        }
        get isEdited() {
            const {activeField: e} = De.getState();
            return !!e && this._isFieldOrFormEdited(e)
        }
    }
    class bt {
        constructor() {
            this.version = 2,
            this._dispatchSaveError = e=>{
                e && (e.disabled = !0);
                document.dispatchEvent(new CustomEvent("OPButtonEvent",{
                    detail: {
                        header: "We aren???t able to save your item",
                        body: [{
                            text: "Something is preventing us from saving your item."
                        }, {
                            text: "Make sure 1Password in your browser is up to date",
                            url: "https://support.1password.com/update-1password-browser/"
                        }, {
                            text: "or contact support for this site."
                        }],
                        error: !0
                    }
                }));
            }
            ,
            this._handleSaveItemRequest = ()=>{
                const e = 2 === this.version ? this.saveButton : document.querySelector("input[data-onepassword-save-request]");
                if (!e)
                    throw new Error("No Save Item Request found.");
                const t = e.getAttribute("data-onepassword-type")
                  , n = !!document.body.getAttribute("data-onepassword-public-key")
                  , r = e.value;
                "string" == typeof t && wt(t) ? M.handleSaveItemRequest(t, r, n) : this._dispatchSaveError(this.saveButton ? this.saveButton : null);
            }
            ,
            this.init(),
            document.addEventListener("OPButtonAdded", (()=>{
                this.init();
            }
            ));
        }
        init() {
            this.setSaveButtonAndVersion(),
            this.setPublicKey(),
            this.registerButton();
        }
        setSaveButtonAndVersion() {
            var e, t;
            this.version = 2,
            this.saveButton = null === (t = null === (e = document.querySelector("onepassword-save-button")) || void 0 === e ? void 0 : e.shadowRoot) || void 0 === t ? void 0 : t.querySelector("button[data-onepassword-save-button]"),
            this.saveButton || (this.saveButton = document.querySelector("button[data-onepassword-save-button]"),
            this.version = 1);
        }
        async setPublicKey() {
            var e;
            if (null === (e = this.saveButton) || void 0 === e ? void 0 : e.hasAttribute("data-onepassword-provide-public-key")) {
                const e = await M.getSaveObjectPublicKey();
                document.body.setAttribute("data-onepassword-public-key", btoa(JSON.stringify(e)));
            }
        }
        registerButton() {
            this.saveButton && (this.saveButton.disabled = !1,
            this.setModalContent(this.saveButton),
            this.saveButton.addEventListener("click", this._handleSaveItemRequest));
        }
        setModalContent(e) {
            if (2 !== this.version)
                return;
            const t = document.querySelector("onepassword-save-button")
              , n = null == t ? void 0 : t.getAttribute("data-onepassword-type");
            !wt(n) && e ? this._dispatchSaveError(e) : document.dispatchEvent(new CustomEvent("OPButtonEvent",{
                detail: {
                    header: "What's Save in 1Password?",
                    body: [{
                        text: `Save this ${null == n ? void 0 : n.replace("-", " ")} in 1Password automatically.`
                    }]
                }
            }));
        }
    }
    function wt(e) {
        return ["login", "identity", "credit-card"].includes(e)
    }
    class Pt {
        constructor(e) {
            this._isListeningForScrollAndResizeEvents = !1,
            this._isCollectingForPotentialLoginFormSubmission = !1,
            this.onActiveInputKeyDown = (e,t)=>{
                if ("ArrowDown" === e) {
                    const {inlineMenuOpen: e, locked: n} = De.getState();
                    e && !n ? this.inlineMenuManager.focus() : this.inlineMenuManager.toggle(t, !!e || void 0);
                } else
                    this._handleKeyDown(e, t);
            }
            ,
            this.onActiveInputChange = e=>{
                De.getState().inlineMenuOpen && this.inlineMenuManager.filter(e.value);
            }
            ,
            this.onButtonClick = e=>{
                this.inlineMenuManager.toggle(e);
            }
            ,
            this.onActiveInputFocus = e=>{
                var t;
                this._onActiveInputFocus();
                const {autoshowMenu: n, canRequestUnlock: r, locked: i} = De.getState();
                !n || i && !r || (null === (t = De.getState().activeField) || void 0 === t ? void 0 : t.hasAttribute(de)) || this.inlineMenuManager.attach(e);
            }
            ,
            this.onActiveInputBlur = ()=>{
                De.dispatch({
                    type: "set-active-field",
                    payload: void 0
                }),
                this.inlineMenuManager.detach();
            }
            ,
            this.collectFrame = e=>{
                const t = De.getState().activeField;
                return C(t ? Object.assign({
                    activeFieldOpid: (n = t,
                    x(document).findIndex((e=>e === n))),
                    activeFormOnly: !0
                }, e) : e);
                var n;
            }
            ,
            this._onScrollOrResize = ()=>{
                this._removeScrollAndResizeEventListeners();
            }
            ,
            this._addScrollAndResizeEventListeners = ()=>{
                this._isListeningForScrollAndResizeEvents || (window.addEventListener("scroll", this._onScrollOrResize, !0),
                window.addEventListener("resize", this._onScrollOrResize, !0),
                this._isListeningForScrollAndResizeEvents = !0);
            }
            ,
            this._removeScrollAndResizeEventListeners = ()=>{
                this._isListeningForScrollAndResizeEvents && (M.removeScrollAndResizeEventListeners(),
                window.removeEventListener("scroll", this._onScrollOrResize, !0),
                window.removeEventListener("resize", this._onScrollOrResize, !0),
                this._isListeningForScrollAndResizeEvents = !1,
                this.onActiveInputBlur());
            }
            ,
            this.handlePotentialLoginFormSubmission = ()=>{
                if (this._isCollectingForPotentialLoginFormSubmission)
                    return;
                this._isCollectingForPotentialLoginFormSubmission = !0;
                const e = this.collectFrame()
                  , t = e.fields.some((e=>"password" === e.type))
                  , n = e.fields.some((e=>e.isUserEdited));
                this._isCollectingForPotentialLoginFormSubmission = !1,
                t && n && M.notifyAboutPotentialLoginSubmission(e);
            }
            ,
            this.saveItemRequestManager = new bt,
            this.formManager = new gt,
            this.formManager.onActiveInputFocus = this.onActiveInputFocus,
            this.formManager.onActiveInputBlur = this.onActiveInputBlur,
            this.formManager.onActiveInputKeyDown = this.onActiveInputKeyDown,
            this.formManager.onButtonClick = this.onButtonClick,
            this.formManager.onActiveInputChange = this.onActiveInputChange,
            M.onHostAppRestarted((()=>this.getFrameManagerConfiguration())),
            this.getFrameManagerConfiguration(),
            (null == e ? void 0 : e.configurePotentialLoginHandlers) && this.setupPotentialLoginListeners(),
            M.onInlineMenuOpened((()=>{
                De.dispatch({
                    type: "set-inline-menu-open",
                    payload: !0
                });
            }
            )),
            M.onInlineMenuClosed((()=>{
                De.dispatch({
                    type: "set-inline-menu-open",
                    payload: !1
                });
            }
            )),
            M.onAddScrollAndResizeEventListeners(this._addScrollAndResizeEventListeners),
            M.onRemoveScrollAndResizeEventListeners(this._removeScrollAndResizeEventListeners),
            M.onLockStateChanged((({locked: e})=>{
                De.dispatch({
                    type: "lock-state-changed",
                    payload: e
                });
            }
            )),
            M.onPageExcluded((e=>{
                De.dispatch({
                    type: "set-page-excluded",
                    payload: e
                });
            }
            )),
            M.onCollectFrameDetails((e=>{
                if (e.frameUrl && window.location.href !== e.frameUrl)
                    throw "Message not addressed to for frame.";
                return this.collectFrame(e.options)
            }
            )),
            M.onPerformFill((e=>{
                var t;
                De.dispatch({
                    type: "fill-start",
                    payload: "inProgress" === e.status
                }),
                null === (t = this._detachInlineMenu) || void 0 === t || t.call(this),
                (e=>{
                    const t = [];
                    if ("undefined" != typeof uuid && "" !== uuid)
                        for (const n of e)
                            t.push(pe(uuid, n));
                    else
                        console.error("Failed to fillFrame: Frame is missing uuid.");
                    return Promise.all(t).then((()=>{}
                    ))
                }
                )(e.response.fillPayload.frames || []).then((()=>{
                    De.dispatch({
                        type: "fill-end"
                    });
                }
                ));
            }
            )),
            M.onPing((()=>{
                M.sayHello();
            }
            )),
            M.onFocusPage((()=>{
                const e = De.getState().activeField;
                e && e.focus();
            }
            )),
            _e("locked", (e=>{
                var t;
                !e && De.getState().activeField && (document.activeElement !== De.getState().activeField && (null === (t = De.getState().activeField) || void 0 === t || t.focus()),
                this.onActiveInputFocus(this.formManager.isEdited));
            }
            ));
        }
        getFrameManagerConfiguration() {
            M.getFrameManagerConfiguration().then((e=>{
                De.dispatch({
                    type: "configure",
                    payload: e
                });
            }
            ));
        }
        setupPotentialLoginListeners() {
            window.addEventListener("click", (e=>{
                var t;
                if (!e.isTrusted)
                    return;
                const n = e.target instanceof HTMLInputElement || e.target instanceof HTMLButtonElement ? e.target : null
                  , r = null !== (t = null == n ? void 0 : n.tagName.toLowerCase()) && void 0 !== t ? t : "";
                "button" !== r && "input" !== r || "submit" !== (null == n ? void 0 : n.type) ? e.target instanceof HTMLElement && (e.target.closest("[type='submit']") || e.target.closest("[role='button']")) && this.handlePotentialLoginFormSubmission() : this.handlePotentialLoginFormSubmission();
            }
            ), !0),
            window.addEventListener("keydown", (e=>{
                if (!e.isTrusted || "Enter" !== e.key)
                    return;
                const t = e.target instanceof HTMLInputElement ? e.target : null;
                "text" !== (null == t ? void 0 : t.type) && "password" !== (null == t ? void 0 : t.type) && "email" !== (null == t ? void 0 : t.type) || this.handlePotentialLoginFormSubmission();
            }
            ), !0),
            window.addEventListener("submit", this.handlePotentialLoginFormSubmission, !0);
        }
    }
    class kt {
        constructor() {
            this.draw = (e,t)=>{
                this.createShadowRoot(),
                this.createDialog(),
                this.setDialogSrc(e, t),
                this.addShadowHostToDOM();
            }
            ,
            this.erase = ()=>{
                var e;
                (null === (e = this.shadowRoot) || void 0 === e ? void 0 : e.host.parentElement) && document.body.removeChild(this.shadowRoot.host);
            }
            ,
            this.createShadowRoot = ()=>{
                !this.shadowRoot && document.body && (this.shadowRoot = document.createElement("com-1password-save-dialog").attachShadow({
                    mode: "closed"
                }));
            }
            ,
            this.createDialog = ()=>{
                this.shadowRoot && !this.dialog && (this.dialog = document.createElement("iframe"),
                this.dialog.id = "op-save-dialog",
                this.dialog.style.cssText = "\n        all: initial;\n        position: fixed;\n        z-index: 2147483647;\n        top: 0px;\n        left: 0px;\n        min-width: 100vw;\n        width: 100vw;\n        max-width: 100vw;\n        min-height: 100vh;\n        height: 100vh;\n        max-height: 100vh;\n        border: none;\n      ",
                this.shadowRoot.appendChild(this.dialog));
            }
            ,
            this.setDialogSrc = (e,t)=>{
                if (!this.dialog)
                    return;
                const {basePath: n, locale: r} = De.getState();
                let i;
                const o = new URLSearchParams({
                    language: r,
                    sessionId: e.toString()
                });
                switch (t.name) {
                case "save-item":
                    i = `${n}/save-dialog/save-dialog.html`;
                    break;
                case "privacy-enable-integration":
                    i = `${n}/save-dialog/privacy-enable-dialog.html`;
                    break;
                case "privacy-create-credit-card":
                    i = `${n}/save-dialog/privacy-create-dialog.html`,
                    o.set("frameIdentifier", t.data.frameIdentifier);
                }
                const a = new URL(i);
                a.search = o.toString(),
                this.dialog.src = a.toString();
            }
            ;
        }
        addShadowHostToDOM() {
            this.shadowRoot && !this.shadowRoot.host.parentElement && document.body.appendChild(this.shadowRoot.host);
        }
    }
    class St {
        constructor() {
            this._add = (e,t)=>{
                this.saveDialogFrame.draw(e, t);
            }
            ,
            this._remove = ()=>{
                this.saveDialogFrame.erase();
            }
            ,
            this.saveDialogFrame = new kt,
            M.onShowSaveDialog((({sessionId: e, type: t})=>{
                this._add(e, t),
                M.removeInlineMenu();
            }
            )),
            M.onHideSaveDialog((()=>{
                this._remove();
            }
            ));
        }
    }
    class Et {
        constructor() {
            this.draw = e=>{
                this.isHidden() ? this.show() : (this.createShadowRoot(),
                this.createMenu(),
                this.setMenuSrc(e),
                this.positionMenu({
                    dir: e.dir,
                    x: e.x,
                    y: e.y
                }),
                this.addShadowHostToDOM(),
                M.sendInlineMenuOpened());
            }
            ,
            this.erase = ()=>{
                var e;
                (null === (e = this.shadowRoot) || void 0 === e ? void 0 : e.host.parentElement) && (document.body.removeChild(this.shadowRoot.host),
                M.sendInlineMenuClosed());
            }
            ,
            this.show = ()=>{
                var e;
                (null === (e = this.menu) || void 0 === e ? void 0 : e.parentNode) && "visible" !== this.menu.style.visibility && (this.menu.style.visibility = "visible");
            }
            ,
            this.hide = ()=>{
                var e;
                (null === (e = this.menu) || void 0 === e ? void 0 : e.parentNode) && "hidden" !== this.menu.style.visibility && (this.menu.style.visibility = "hidden");
            }
            ,
            this.isHidden = ()=>{
                var e, t, n;
                return null !== (t = null === (e = this.menu) || void 0 === e ? void 0 : e.parentElement) && void 0 !== t && t && "hidden" === (null === (n = this.menu) || void 0 === n ? void 0 : n.style.visibility)
            }
            ,
            this.resize = (e,t)=>{
                var n;
                (null === (n = this.menu) || void 0 === n ? void 0 : n.parentNode) && (this.menu.style.height = `${e}px`,
                t && (this.menu.style.width = `${t}px`));
            }
            ,
            this.focus = ()=>{
                var e;
                (null === (e = this.menu) || void 0 === e ? void 0 : e.parentNode) && (this.isHidden() && this.show(),
                M.focusInlineMenu(),
                this.menu.focus());
            }
            ,
            this.createShadowRoot = ()=>{
                !this.shadowRoot && document.body && (this.shadowRoot = document.createElement("com-1password-menu").attachShadow({
                    mode: "closed"
                }));
            }
            ,
            this.createMenu = ()=>{
                this.shadowRoot && !this.menu && (this.menu = document.createElement("iframe"),
                this.menu.id = "op-menu",
                this.shadowRoot.appendChild(this.menu));
            }
            ,
            this.setMenuSrc = e=>{
                if (!this.menu)
                    return;
                const {fieldAnalysis: t, context: n, edited: r, locale: i, frameIdentifier: o, showOptionsMenu: a, appTheme: u} = e
                  , {fieldDesignation: c, formDesignation: s, suggestions: l} = t
                  , {basePath: d, inlineMenuToken: f, locked: p} = De.getState();
                if ("string" != typeof f)
                    throw new Error("No inline menu token exists.");
                const v = new URL(`${d}/menu/menu.html`)
                  , h = new URLSearchParams({
                    frameIdentifier: o,
                    token: f,
                    displayHost: window.location.hostname,
                    fieldDesignation: c,
                    formDesignation: s,
                    suggestions: JSON.stringify(l),
                    edited: r.toString(),
                    locked: p.toString(),
                    locale: i,
                    showOptionsMenu: a.toString(),
                    appTheme: u
                });
                n && h.append("context", n),
                v.search = h.toString(),
                this.menu.src = v.toString();
            }
            ,
            this.positionMenu = ({dir: e, x: t, y: n})=>{
                this.menu && (this.menu.style.cssText = `\n        all: initial;\n        position: fixed;\n        top: ${n}px;\n        ${"rtl" === e ? "right" : "left"}: ${t}px;\n        z-index: 2147483647;\n        min-width: 250px;\n        max-width: 350px;\n        min-height: 0px;\n        height: 242px;\n        max-height: 242px;\n        visibility: hidden;\n        border: none;\n        border-radius: 6px;\n        box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.15), 0 2px 24px rgba(0, 0, 0, 0.07);\n        outline: 0;\n      `);
            }
            ;
        }
        addShadowHostToDOM() {
            if (!this.shadowRoot || this.shadowRoot.host.parentElement)
                return;
            const e = document.getElementsByTagName("com-1password-save-dialog")[0];
            e ? document.body.insertBefore(this.shadowRoot.host, e) : document.body.appendChild(this.shadowRoot.host);
        }
    }
    class Ft {
        constructor() {
            this.attach = (e=!1,t=!1)=>{
                const n = this.getInlineMenuConfiguration(e, t);
                n && this._attach(n);
            }
            ,
            this.detach = ()=>{
                this._detach();
            }
            ,
            this.toggle = (e,t)=>{
                const {inlineMenuOpen: n, locked: r} = De.getState()
                  , i = void 0 === t ? !n : t;
                De.dispatch({
                    type: "set-autoshow-menu",
                    payload: i
                }),
                i ? r && !0 === t ? M.requestManagedUnlock().then((t=>{
                    t || this.attach(e, !0);
                }
                )) : this.attach(e, !0) : this.detach();
            }
            ,
            this.focus = ()=>{
                this._focus();
            }
            ,
            this.filter = e=>{
                M.filterInlineMenu(e);
            }
            ,
            this.getInlineMenuConfiguration = (e,t)=>{
                const n = this.getMenuPosition();
                if (!n)
                    return;
                const r = De.getState().activeFieldAnalysis;
                if (!r)
                    return;
                if (r.requiresInteractionForMenu && !t)
                    return;
                const {locale: i, frameIdentifier: o, showOptionsMenu: a, appTheme: u} = De.getState()
                  , c = function() {
                    if ("accounts.google.com" !== window.location.host)
                        return;
                    const e = C().fields.find((e=>"email" === e.type && "identifier" === e.htmlName && "hiddenEmail" === e.htmlId));
                    return e && e.value
                }();
                return Object.assign(Object.assign({}, n), {
                    fieldAnalysis: r,
                    edited: e,
                    context: c,
                    locale: i,
                    frameIdentifier: o,
                    showOptionsMenu: a,
                    appTheme: u
                })
            }
            ,
            this.getMenuPosition = ()=>{
                const {activeFieldRect: e} = De.getState();
                if (!e)
                    return;
                const {top: t, left: n, right: r, height: i} = e
                  , {activeFieldDir: o} = De.getState();
                return {
                    dir: o,
                    x: "rtl" === o ? window.innerWidth - r : n,
                    y: t + i
                }
            }
            ,
            this.handleInlineMenuPosition = ({configuration: e, matchableFrameWindowProps: t})=>{
                const n = document.getElementsByTagName("iframe");
                if (0 === n.length)
                    return;
                const r = this.findMostEqualFrame([...n], t);
                if (0 === r.confidence)
                    return;
                const {top: i, left: o, right: a} = r.frame.getBoundingClientRect();
                this._attach(Object.assign(Object.assign({}, e), {
                    x: e.x + ("rtl" === e.dir ? t.width - a : o),
                    y: e.y + i
                }));
            }
            ,
            this.findMostEqualFrame = (e,t)=>e.reduce(((e,n)=>{
                const {width: r, height: i, src: o, name: a} = t
                  , u = Math.abs(n.clientWidth - r) <= 1 && Math.abs(n.clientHeight - i) <= 1
                  , c = n.src && o && n.src === o
                  , s = n.name && a && n.name === a;
                let l = 0;
                return u && c && s ? l = 3 : u && (c || s) || c && s ? l = 2 : u && (l = 1),
                e.push({
                    frame: n,
                    confidence: l
                }),
                e
            }
            ), []).sort(((e,t)=>t.confidence - e.confidence))[0],
            M.onForwardInlineMenuPosition(this.handleInlineMenuPosition),
            M.onGetFrameOrigin((e=>{
                const {frameIdentifier: t} = De.getState();
                e.frameIdentifier === t && M.sendFrameOrigin(e.uuid);
            }
            ));
        }
    }
    class It extends Ft {
        constructor() {
            super(),
            this._focus = ()=>{
                setTimeout((()=>{
                    this.inlineMenuFrame.focus();
                }
                ), 1);
            }
            ,
            this._attach = e=>{
                this.inlineMenuFrame.draw(e);
            }
            ,
            this._detach = ()=>{
                this.inlineMenuFrame.erase();
            }
            ,
            this.inlineMenuFrame = new Et,
            this.shouldShowAfterDragEnds = !1,
            De.dispatch({
                type: "set-inline-menu-token",
                payload: window.crypto.getRandomValues(new Uint32Array(1))[0].toString(36)
            }),
            M.onRequestVerificationToken((()=>{
                M.sendVerificationToken(De.getState().inlineMenuToken);
            }
            )),
            M.onUnlockRequested((()=>{
                M.requestManagedUnlock();
            }
            )),
            M.onResizeInlineMenu((({height: e, width: t})=>{
                this.inlineMenuFrame.resize(e, t);
            }
            )),
            M.onItemDetailValueDragStart((()=>{
                this.inlineMenuFrame.isHidden() || (this.shouldShowAfterDragEnds = !0),
                this.inlineMenuFrame.hide(),
                De.dispatch({
                    type: "set-inline-menu-open",
                    payload: !1
                });
            }
            )),
            M.onItemDetailValueDragEnd((()=>{
                this.shouldShowAfterDragEnds && this.inlineMenuFrame.show(),
                this.shouldShowAfterDragEnds = !1;
            }
            )),
            M.onInlineMenuReady((()=>{
                this.inlineMenuFrame.show();
            }
            )),
            M.onRemoveInlineMenu((e=>{
                this._detach(),
                e && De.dispatch({
                    type: "set-autoshow-menu",
                    payload: !1
                });
            }
            )),
            M.onFocusInlineMenuFrame(this._focus),
            _e("locked", (e=>{
                e && this._detach();
            }
            )),
            _e("inlineDisabled", (e=>{
                e && this._detach();
            }
            ));
        }
    }
    class At extends Pt {
        constructor(e) {
            super(e),
            this._detachInlineMenu = ()=>{
                this.inlineMenuManager.detach();
            }
            ,
            this._handleKeyDown = (e,t)=>{
                switch (e) {
                case "ArrowUp":
                case "Escape":
                    this.inlineMenuManager.toggle(t, !1);
                }
            }
            ,
            this._onActiveInputFocus = ()=>{
                this._addScrollAndResizeEventListeners();
            }
            ,
            this.inlineMenuManager = new It,
            this.saveDialogManager = new St;
            const t = function(e) {
                if (!w(e))
                    throw new Error(`Not able to support locale ${e}`);
                const t = P(e)
                  , n = g();
                return n.set({
                    locale: e,
                    messages: t
                }),
                n
            }(De.getState().locale);
            M.onRequestFillAuthorization((({type: e})=>{
                const {host: n, href: r} = window.location;
                let i;
                switch (e) {
                case "Privacy":
                    i = t.lookup("authorize-fill-privacy", {
                        host: n
                    });
                    break;
                default:
                    i = t.lookup("authorize-fill", {
                        host: n
                    });
                }
                return {
                    url: r,
                    authorized: window.confirm(i)
                }
            }
            )),
            M.onKeyDown((({key: e, formEdited: t})=>this._handleKeyDown(e, t))),
            M.pageReady(),
            M.transport.on("show-save-prompt", (e=>{
                if (document.getElementById("b5x-save-prompt"))
                    return;
                const t = document.createElement("iframe");
                return t.id = "b5x-save-prompt",
                e && (t.src = e),
                document.body.appendChild(t),
                t
            }
            )),
            M.transport.on("close-save-prompt", (e=>{
                const t = document.querySelector(`iframe[src="${e}"]`);
                t && document.body.removeChild(t);
            }
            ));
        }
    }
    class Mt extends Ft {
        constructor() {
            super(...arguments),
            this._attach = e=>{
                M.forwardInlineMenuPosition(e, {
                    width: window.innerWidth,
                    height: window.innerHeight,
                    src: window.location.href,
                    name: window.name
                });
            }
            ,
            this._detach = ()=>{
                M.removeInlineMenu();
            }
            ,
            this._focus = ()=>{
                M.focusInlineMenuFrame();
            }
            ;
        }
    }
    class Ot extends Pt {
        constructor(e) {
            super(e),
            this._handleKeyDown = (e,t)=>{
                switch (e) {
                case "ArrowUp":
                case "Escape":
                    M.sendKeyDown(e, t);
                }
            }
            ,
            this._onActiveInputFocus = ()=>{
                this._addScrollAndResizeEventListeners(),
                M.addScrollAndResizeEventListeners();
            }
            ,
            this.inlineMenuManager = new Mt;
        }
    }
    ht = {
        configurePotentialLoginHandlers: !0
    },
    window === window.top ? new At(ht) : new Ot(ht);

}());
