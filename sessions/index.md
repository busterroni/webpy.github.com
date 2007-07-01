---
layout: default
title: Sessions (GSoC)
---

# Sessions (GSoC)

# Spec
## Summary
The main session object will be created depending on the configuration. However the user/developer will choose it's usage. If needed the user will call start() method and there by inicializing handlers, variables from db-config like variables. The session will be implemented as storage object. Session identification will mostly rely on client cookies (optionally also on client IP address). All data will be stored/retrieved through handler object (DB-, file-, cookie?- based). Save/commit & destroy methods will be created. The user will have the option to set various option including expiration timeout, handler, session id generator (a default will be provided).

## Design
The session functionality will rely on a two-layer implementation: Session -> Handler. The Session class will provide identification & verification of the client request while the Handler-like classes will guarante data persistence.

## Implementation details

The Session class is a derivate of Storage. It will include these "public" methods:

 * start() - it will start the session, regenerate id, set cookies, retreive data if the session isn't new; it will call \_identity(), \_verify(), \_generator(), \_retreive()
 * get_id() - it will return current session id
 * cleanup() - it will clean expired sessions depending on the provided interface by choosen Handler object (cookie based will do nothing); it will call Handler.clean() with the user preset timeout
 * save() - it will save session data using the _store()
 * destroy() - it will remove the session (cookies & data); it will call _remove()

The Session class will include these "private" methods:

 * _generate\_id() - it will be an implicit session id generator; it will probably _only_ make a hash of ip, time, seed, microtime
 * _identify() - it will identify the session id (through client cookie                s)
 * _verify() - it will verify the session id with retreived data from handler object i.e. check for expiration, IP change (headers change?)
 * _store() - a simple wrapper around Handler.store(); data will be passed **unpickled**
 * _retreive() - a simple wrapper around Handler.retreive(); data will be awaited **unpickled**
 * _remove() - a simple wrapper around Handler.remove()

The Session class will depend on web.config.session_parameters to better specify developer requirements:

 * timeout - number of second after a not-updated session will be considered expired; default value: 600
 * id_seed - a seed-string that will be used in the default Session._generator(); default value: 'web.py'
 * regenerate_id - should the session id be regenerated and set again with a cookie on every request?; default value: True
 * generator - a function to generate _random_ session ids, if False, the implicit generator (Session.\_generate\_id()) will be used; default value: False
 * ignore_change_ip - if the pair ( id, ip ) doesn't match the retreived data from Handler objcet, then fail/raise exception/...; default value: False
 * ignore_expiration - should the session expiration be ignored?; default value: False
 * handler - a Handler-like object to provide persistence for Session class; default value: DBHandler()

TODO:
    web.config.handler_parameters : {
        file_dir : '/tmp',
        db_table : 'session_data' # table name
    } # optional handler settings
  Handler
  DBHandler

## Notes
 * data will be stored in Session object member variable _data and passed as Storage variables to Handler object (internally in a Handler class they will be stored as "pickled" data)
 * session id will be just a hash of some variables ([semi]random) and a seed
 * Session object will use web.ctx.session_parameters
 * DBHandler will need an extra table in the db
 * [DBHandler specs](/sessions/dbhandler)
 * [prototype](/sessions/prototype)
 * [Google Summer of Code project](/sessions/gsoc)
